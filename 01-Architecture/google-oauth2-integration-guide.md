# Google OAuth 2.0 — Алгоритм интеграции с Spring Security

#architecture #spring-boot #oauth2 #google #oidc #security #java

Руководство по настройке входа через Google в Spring Boot приложении. Основано на успешной реализации в проекте Montblanc.

---

## 1. Подготовка в Google Cloud Console (console.cloud.google.com)

### 1.1 Создание проекта
- Зайти в **APIs & Services** → **Credentials**
- Создать проект или выбрать существующий

### 1.2 OAuth consent screen
- **APIs & Services** → **OAuth consent screen**
- Выбрать **External** (или Internal для G Workspace)
- Заполнить: App name, User support email, Developer contact
- **Scopes:** добавить `email`, `profile`, `openid` (или оставить по умолчанию)

### 1.3 Создание учётных данных
- **Credentials** → **Create Credentials** → **OAuth client ID**
- **Application type:** Web application
- **Name:** произвольное (например, "Montblanc Web")
- **Authorized redirect URIs:** `https://ВАШ_ДОМЕН/login/oauth2/code/google`
- Получить **Client ID** и **Client Secret**

---

## 2. Важные особенности Google OAuth 2.0

| Особенность | Описание |
|---|---|
| **OIDC** | Google поддерживает OpenID Connect — Spring Boot знает endpoints по умолчанию |
| **PKCE** | Не обязателен, но поддерживается (можно использовать для всех провайдеров) |
| **User info** | Плоский JSON: `sub`, `email`, `name`, `picture` — DefaultOAuth2UserService подходит |
| **Scopes** | `openid` обязателен, `profile` и `email` — для имени и почты |
| **Cross-site redirect** | Cookie сессии при редиректе с accounts.google.com — SameSite=None, Secure |

---

## 3. Конфигурация Spring Boot

### 3.1 application.properties

```properties
# Прокси: для корректного redirect_uri при работе за nginx
server.forward-headers-strategy=framework

# Сессия: cookie при cross-site redirect с accounts.google.com
server.servlet.session.cookie.same-site=none
server.servlet.session.cookie.secure=true
server.servlet.session.cookie.http-only=true

# Google OAuth2 — Spring Boot автоматически подставляет стандартные URIs
spring.security.oauth2.client.registration.google.client-id=ВАШ_CLIENT_ID.apps.googleusercontent.com
spring.security.oauth2.client.registration.google.client-secret=ВАШ_CLIENT_SECRET
spring.security.oauth2.client.registration.google.scope=openid,profile,email
spring.security.oauth2.client.registration.google.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}
```

**Ключевые моменты:**
- Провайдер `google` предустановлен в Spring Security — URIs подставляются автоматически
- `client-authentication-method` по умолчанию `client_secret_post` — Google принимает
- `redirect-uri` с `{baseUrl}` — Spring подставит текущий хост (важно за прокси)

### 3.2 Scopes

| Scope | Назначение |
|---|---|
| `openid` | Обязателен для OIDC, идентификатор пользователя (`sub`) |
| `profile` | Имя, аватар (`name`, `picture`) |
| `email` | Email пользователя |

---

## 4. SecurityConfig — без доп. настроек для Google

Google работает с дефолтной конфигурацией Spring Security. Кастомный resolver нужен только если в проекте есть Twitter (PKCE). Для одного Google достаточно:

```java
http
    .oauth2Login(oauth2 -> oauth2
            .userInfoEndpoint(userInfo -> userInfo
                    .userService(oauth2UserService())  // DefaultOAuth2UserService для google
            )
            .successHandler(googleOAuth2SuccessHandler)
    );
```

`DefaultOAuth2UserService` корректно обрабатывает ответ Google UserInfo.

---

## 5. UserService — DefaultOAuth2UserService

Google возвращает плоский JSON:

```json
{
  "sub": "123456789",
  "name": "User Name",
  "given_name": "User",
  "family_name": "Name",
  "picture": "https://...",
  "email": "user@gmail.com",
  "email_verified": true
}
```

`DefaultOAuth2UserService` парсит его без доработок. Кастомный сервис не нужен.

---

## 6. Success Handler — ветка для Google

В `AuthenticationSuccessHandler` для `provider=google`:

```java
if ("twitter".equals(provider)) {
    // X (Twitter) — см. X_OAUTH2_INTEGRATION_GUIDE.md
} else {
    // Google: email, name из атрибутов
    email = (String) attributes.get("email");
    name = (String) attributes.get("name");
    if (email == null || email.isBlank()) {
        email = oauth2User.getName();  // fallback на sub
    }
    if (name == null || name.isBlank()) {
        name = email;
    }
    email = email.trim().toLowerCase();
    name = name != null ? name.trim() : email;
    passwordPlaceholder = "OAUTH_GOOGLE";
}
```

---

## 7. Frontend

Ссылка на авторизацию (полный редирект на backend):

```html
<a href="/oauth2/authorization/google">Sign in with Google</a>
```

Spring сформирует URL: `https://accounts.google.com/o/oauth2/v2/auth?client_id=...&redirect_uri=...&scope=openid%20profile%20email&response_type=code`

---

## 8. Чеклист перед деплоем

- [ ] Redirect URI в Google Console точно совпадает с `https://ДОМЕН/login/oauth2/code/google`
- [ ] `server.forward-headers-strategy=framework` (если за nginx)
- [ ] Cookie: `same-site=none`, `secure=true` (HTTPS обязателен)
- [ ] Client Secret в переменных окружения для production (не в коде)
- [ ] OAuth consent screen в статусе Published (или Testing с добавленными test users)

---

## 9. Типичные ошибки и решения

| Ошибка | Причина | Решение |
|---|---|---|
| **redirect_uri_mismatch** | URI не совпадает | Добавить точный URI в Authorized redirect URIs |
| **access_denied** | Пользователь отменил | Обработать редирект на `/login?error` |
| **invalid_client** | Неверный client_id/secret | Проверить Credentials в Google Console |
| **authorization_request_not_found** | Сессия теряется при redirect | `same-site=none`, `secure=true` |
| **403 / blocked** | Consent screen не настроен | Настроить OAuth consent screen, добавить scopes |
| **Редирект на старый домен / белый экран** | `APP_BASE_URL` указывает на backend/старый хост | В `SuccessHandler` фильтровать `originHost` и направлять на URL decoupled SPA (Vercel) |
| **Ambiguous mapping crash** | Дублирование `@GetMapping("/api/me")` в разных контроллерах | Оставить эндпоинт в `AuthController`, расширив `UserSessionDTO` (email, isAdmin) |

---

## 10. Порядок действий (краткий алгоритм)

1. **Google Cloud Console:** создать проект, OAuth consent screen, OAuth client ID (Web application)
2. **Authorized redirect URIs:** `https://ДОМЕН/login/oauth2/code/google`
3. **application.properties:** `client-id`, `client-secret`, `scope=openid,profile,email`
4. **application.properties:** cookie `same-site=none`, `secure=true`; `forward-headers-strategy`
5. **SecurityConfig:** `oauth2Login` с `successHandler` (кастомный или `SimpleUrlAuthenticationSuccessHandler`)
6. **SuccessHandler:** ветка для `provider=google` (email, name из attributes)
7. **Frontend:** ссылка `/oauth2/authorization/google`
8. **Деплой:** HTTPS, переменные окружения, проверить Redirect URI

---

## 11. Сравнение с X (Twitter)

| Аспект | Google | X (Twitter) |
|---|---|---|
| **UserService** | `DefaultOAuth2UserService` | Кастомный `TwitterOAuth2UserService` |
| **PKCE** | Опционально | **Обязателен** |
| **client-authentication-method** | `client_secret_post` (по умолчанию) | `client_secret_basic` |
| **Provider URIs** | Авто (OIDC discovery) | Задавать вручную |
| **Формат user info** | Плоский JSON | Вложенный `{ "data": {...} }` |
| **Email** | Есть в атрибутах | Нет — генерировать `username@x.local` |

---

*Документ создан на основе реализации в проекте Montblanc (2026).*
