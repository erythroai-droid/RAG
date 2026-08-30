# X (Twitter) OAuth 2.0 — Алгоритм интеграции с Spring Security

#architecture #spring-boot #oauth2 #twitter-x #pkce #security #java

Руководство по настройке входа через X (Twitter) в Spring Boot приложении. Основано на успешной реализации в проекте Montblanc.

---

## 1. Подготовка в X Developer Portal (console.x.com)

### 1.1 Создание приложения
- Зайти в **Apps** → создать или выбрать приложение
- **Type of App:** Web App, Automated App or Bot (Confidential client)

### 1.2 User authentication settings (Edit settings)
- **Callback URI / Redirect URL:** `https://ВАШ_ДОМЕН/login/oauth2/code/twitter`
- **Website URL:** `https://ВАШ_ДОМЕН`
- App permissions (OAuth 1.0a) — не влияют на OAuth 2.0, но можно выбрать Read and write

### 1.3 Получить ключи
- **OAuth 2.0 Keys:** Client ID, Client Secret
- Client Secret хранить в переменной окружения `TWITTER_CLIENT_SECRET` (production)

---

## 2. Важные особенности X OAuth 2.0

| Особенность | Описание |
|---|---|
| **PKCE обязателен** | Spring Security по умолчанию не отправляет PKCE — нужно добавить программно |
| **Scopes в URL** | Scopes задаются в параметре `scope` при редиректе на authorize, а не в дашборде |
| **Token endpoint** | Требует `client_secret_basic` (Basic auth в заголовке) |
| **User info** | API `/2/users/me` возвращает JSON с вложенным объектом `data` |
| **Cross-site redirect** | Cookie сессии должен отправляться при редиректе с x.com — нужны `SameSite=None`, `Secure` |

---

## 3. Конфигурация Spring Boot

### 3.1 application.properties

```properties
# Прокси: для корректного redirect_uri при работе за nginx
server.forward-headers-strategy=framework

# Сессия: cookie при cross-site redirect с x.com
server.servlet.session.cookie.same-site=none
server.servlet.session.cookie.secure=true
server.servlet.session.cookie.http-only=true

# X (Twitter) OAuth2
spring.security.oauth2.client.registration.twitter.client-id=ВАШ_CLIENT_ID
spring.security.oauth2.client.registration.twitter.client-secret=${TWITTER_CLIENT_SECRET:fallback_secret}
spring.security.oauth2.client.registration.twitter.client-authentication-method=client_secret_basic
spring.security.oauth2.client.registration.twitter.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.twitter.scope=tweet.read,users.read,offline.access
spring.security.oauth2.client.registration.twitter.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}

# X API endpoints (отличаются от стандартного OIDC)
spring.security.oauth2.client.provider.twitter.authorization-uri=https://twitter.com/i/oauth2/authorize
spring.security.oauth2.client.provider.twitter.token-uri=https://api.twitter.com/2/oauth2/token
spring.security.oauth2.client.provider.twitter.user-info-uri=https://api.twitter.com/2/users/me
spring.security.oauth2.client.provider.twitter.user-name-attribute=id
```

**Ключевые моменты:**
- `client-authentication-method=client_secret_basic` — Twitter требует Basic auth при обмене code на token
- `scope` — указывается здесь, Spring добавит в URL авторизации
- `redirect-uri` с `{baseUrl}` — Spring подставит текущий хост (важно за прокси)

### 3.2 Scopes

| Scope | Назначение |
|---|---|
| `users.read` | Профиль пользователя (`/2/users/me`) |
| `tweet.read` | Чтение твитов |
| `offline.access` | Refresh token |

---

## 4. SecurityConfig — PKCE

Spring Security **не добавляет PKCE** для Twitter автоматически. Нужен customizer:

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http, ClientRegistrationRepository clientRegistrationRepository) {
    DefaultOAuth2AuthorizationRequestResolver resolver = new DefaultOAuth2AuthorizationRequestResolver(
            clientRegistrationRepository,
            OAuth2AuthorizationRequestRedirectFilter.DEFAULT_AUTHORIZATION_REQUEST_BASE_URI
    );
    resolver.setAuthorizationRequestCustomizer(this::addPkceParameters);

    http
            .oauth2Login(oauth2 -> oauth2
                    .authorizationEndpoint(auth -> auth
                            .authorizationRequestResolver(resolver)
                    )
                    // ...
            );
    return http.build();
}

private static void addPkceParameters(OAuth2AuthorizationRequest.Builder builder) {
    String codeVerifier = generateCodeVerifier();
    String codeChallenge = createS256CodeChallenge(codeVerifier);

    builder.attributes(attrs -> attrs.put(PkceParameterNames.CODE_VERIFIER, codeVerifier));
    builder.additionalParameters(params -> {
        params.put(PkceParameterNames.CODE_CHALLENGE, codeChallenge);
        params.put(PkceParameterNames.CODE_CHALLENGE_METHOD, "S256");
    });
}

private static String generateCodeVerifier() {
    SecureRandom secureRandom = new SecureRandom();
    byte[] bytes = new byte[96];
    secureRandom.nextBytes(bytes);
    return Base64.getUrlEncoder().withoutPadding().encodeToString(bytes);
}

private static String createS256CodeChallenge(String codeVerifier) {
    try {
        byte[] digest = MessageDigest.getInstance("SHA-256")
                .digest(codeVerifier.getBytes(StandardCharsets.US_ASCII));
        return Base64.getUrlEncoder().withoutPadding().encodeToString(digest);
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
}
```

---

## 5. TwitterOAuth2UserService — формат ответа API

X API `/2/users/me` возвращает:

```json
{
  "data": {
    "id": "123",
    "name": "User Name",
    "username": "username",
    "profile_image_url": "https://..."
  }
}
```

`DefaultOAuth2UserService` ожидает плоский объект. Нужен кастомный сервис:

```java
public class TwitterOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {
    @Override
    public OAuth2User loadUser(OAuth2UserRequest userRequest) {
        String uri = userInfoUri + "?user.fields=id,name,username,profile_image_url";
        // GET с Bearer token
        // Парсинг root.get("data")
        // Маппинг в DefaultOAuth2User с nameAttributeKey="username"
    }
}
```

---

## 6. Success Handler — единый для Google и X

В `AuthenticationSuccessHandler` различать провайдера по `OAuth2AuthenticationToken.getAuthorizedClientRegistrationId()`:

```java
if ("twitter".equals(provider)) {
    // X: id, name, username (email нет — генерируем username@x.local)
    String username = (String) attributes.get("username");
    email = username + "@x.local";
    name = (String) attributes.get("name");
} else {
    // Google: email, name
    email = (String) attributes.get("email");
    name = (String) attributes.get("name");
}
```

---

## 7. Frontend

Ссылка на авторизацию (полный редирект на backend):

```html
<a href="/oauth2/authorization/twitter">Sign in with X</a>
```

Spring сформирует URL: `https://twitter.com/i/oauth2/authorize?client_id=...&redirect_uri=...&scope=...&code_challenge=...&code_challenge_method=S256`

---

## 8. Чеклист перед деплоем

- [ ] Callback URI в X Portal точно совпадает с `https://ДОМЕН/login/oauth2/code/twitter`
- [ ] `server.forward-headers-strategy=framework` (если за nginx)
- [ ] Cookie: `same-site=none`, `secure=true` (HTTPS обязателен)
- [ ] `TWITTER_CLIENT_SECRET` в переменных окружения (не в коде)
- [ ] Scopes указаны в `application.properties`
- [ ] PKCE добавлен в SecurityConfig
- [ ] TwitterOAuth2UserService обрабатывает формат `{ "data": { ... } }`

---

## 9. Типичные ошибки и решения

| Ошибка | Причина | Решение |
|---|---|---|
| **400 "Something went wrong"** | Нет PKCE | Добавить `addPkceParameters` в resolver |
| **authorization_request_not_found** | Сессия теряется при redirect | `same-site=none`, `secure=true` |
| **401 при обмене code на token** | Неверный auth | `client_secret_basic` |
| **403 Unsupported Authentication** | Нет/неверные scopes | Указать `scope=users.read,tweet.read,offline.access` |
| **403 на /2/users/me** | Токен без users.read | Переавторизоваться (новый scope) |
| **invalid_user_info_response** | Формат ответа | Парсить `data`, не корень JSON |

---

## 10. Порядок действий (краткий алгоритм)

1. **X Developer Portal:** создать приложение, настроить Callback URI, получить Client ID/Secret
2. **application.properties:** `client-id`, `client-secret`, `scope`, `redirect-uri`, provider URIs, `client-authentication-method`
3. **application.properties:** cookie `same-site=none`, `secure=true`; `forward-headers-strategy`
4. **SecurityConfig:** `DefaultOAuth2AuthorizationRequestResolver` + `addPkceParameters`
5. **TwitterOAuth2UserService:** кастомный `loadUser` с парсингом `data`
6. **SecurityConfig:** `oauth2UserService()` — роутинг twitter → `TwitterOAuth2UserService`
7. **SuccessHandler:** ветка для `provider=twitter` (email из `username@x.local`)
8. **Frontend:** ссылка `/oauth2/authorization/twitter`
9. **Деплой:** HTTPS, переменные окружения, проверить Callback URI

---

*Документ создан на основе реализации в проекте Montblanc (2026).*
