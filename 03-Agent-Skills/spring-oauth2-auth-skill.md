# 🔐 Скилл: Интеграция Spring Security 6 & OAuth 2.0 (Google + X/Twitter)

#agent-skills #spring-boot #oauth2 #google #twitter-x #security #pkce

> **Назначение:** Пошаговая инструкция для AI-агентов по настройке Spring Boot приложения для авторизации пользователей через Google и X (Twitter) OAuth 2.0.

---

## 1. Пошаговый алгоритм внедрения

### Шаг 1: Настройка X Developer Portal
1. Создать приложение в Developer Portal (тип `Web App, Automated App or Bot`).
2. Включить OAuth 2.0:
   - **Callback URI:** `https://DOMAIN/login/oauth2/code/twitter`
   - **Website URL:** `https://DOMAIN`
3. Скопировать `Client ID` и `Client Secret`. Сохранить секрет в переменной окружения `TWITTER_CLIENT_SECRET`.

### Шаг 2: Настройка Google Cloud Console
1. Создать OAuth Client ID (тип `Web application`).
2. Добавить **Authorized redirect URI:** `https://DOMAIN/login/oauth2/code/google`.
3. Скопировать `Client ID` и `Client Secret`.

### Шаг 3: Конфигурация `application.properties`
```properties
server.forward-headers-strategy=framework
server.servlet.session.cookie.same-site=none
server.servlet.session.cookie.secure=true

# Google
spring.security.oauth2.client.registration.google.client-id=${GOOGLE_CLIENT_ID}
spring.security.oauth2.client.registration.google.client-secret=${GOOGLE_CLIENT_SECRET}
spring.security.oauth2.client.registration.google.scope=openid,profile,email

# Twitter / X
spring.security.oauth2.client.registration.twitter.client-id=${TWITTER_CLIENT_ID}
spring.security.oauth2.client.registration.twitter.client-secret=${TWITTER_CLIENT_SECRET}
spring.security.oauth2.client.registration.twitter.client-authentication-method=client_secret_basic
spring.security.oauth2.client.registration.twitter.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.twitter.scope=users.read,tweet.read,offline.access
spring.security.oauth2.client.registration.twitter.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}
spring.security.oauth2.client.provider.twitter.authorization-uri=https://twitter.com/i/oauth2/authorize
spring.security.oauth2.client.provider.twitter.token-uri=https://api.twitter.com/2/oauth2/token
spring.security.oauth2.client.provider.twitter.user-info-uri=https://api.twitter.com/2/users/me
spring.security.oauth2.client.provider.twitter.user-name-attribute=id
```

### Шаг 4: Регистрация PKCE Customizer
Обязательно подключить генератор PKCE в `SecurityConfig` (см. [[spring-boot-oauth2-architecture|архитектуру Spring Boot]]).

### Шаг 5: Обработка User Info & Success Handler
- Для Google: извлечь `email` и `name`.
- Для X (Twitter): распарсить объект `data`, сгенерировать виртуальный email `username@x.local`, сохранить пользователя в БД.

### Шаг 6: Ссылка на фронтенде
```html
<!-- Авторизация через бэкенд редирект -->
<a href="/oauth2/authorization/google">Войти через Google</a>
<a href="/oauth2/authorization/twitter">Войти через X (Twitter)</a>
```
