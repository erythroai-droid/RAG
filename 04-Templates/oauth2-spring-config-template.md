# 📑 Шаблон: Spring Security OAuth2 Конфигурация (Spring OAuth2 Template)

#templates #spring-boot #oauth2 #security #config

> **Назначение:** Готовый шаблон файлов `application.properties` и `SecurityConfig.java` для развертывания авторизации через Google и X (Twitter) в Spring Boot 3+.

---

## 1. `application.properties` Template

```properties
# ==========================================
# Proxy & Forwarded Headers
# ==========================================
server.forward-headers-strategy=framework

# ==========================================
# Session & Cross-Site Cookies
# ==========================================
server.servlet.session.cookie.same-site=none
server.servlet.session.cookie.secure=true
server.servlet.session.cookie.http-only=true

# ==========================================
# OAuth2 Client: Google
# ==========================================
spring.security.oauth2.client.registration.google.client-id=${GOOGLE_CLIENT_ID:google-client-id}
spring.security.oauth2.client.registration.google.client-secret=${GOOGLE_CLIENT_SECRET:google-secret}
spring.security.oauth2.client.registration.google.scope=openid,profile,email
spring.security.oauth2.client.registration.google.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}

# ==========================================
# OAuth2 Client: X (Twitter)
# ==========================================
spring.security.oauth2.client.registration.twitter.client-id=${TWITTER_CLIENT_ID:twitter-client-id}
spring.security.oauth2.client.registration.twitter.client-secret=${TWITTER_CLIENT_SECRET:twitter-secret}
spring.security.oauth2.client.registration.twitter.client-authentication-method=client_secret_basic
spring.security.oauth2.client.registration.twitter.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.twitter.scope=users.read,tweet.read,offline.access
spring.security.oauth2.client.registration.twitter.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}

spring.security.oauth2.client.provider.twitter.authorization-uri=https://twitter.com/i/oauth2/authorize
spring.security.oauth2.client.provider.twitter.token-uri=https://api.twitter.com/2/oauth2/token
spring.security.oauth2.client.provider.twitter.user-info-uri=https://api.twitter.com/2/users/me
spring.security.oauth2.client.provider.twitter.user-name-attribute=id
```

---

## 2. `SecurityConfig.java` Template

```java
package com.example.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.oauth2.client.registration.ClientRegistrationRepository;
import org.springframework.security.oauth2.client.web.DefaultOAuth2AuthorizationRequestResolver;
import org.springframework.security.oauth2.client.web.OAuth2AuthorizationRequestRedirectFilter;
import org.springframework.security.oauth2.core.endpoint.OAuth2AuthorizationRequest;
import org.springframework.security.oauth2.core.endpoint.PkceParameterNames;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;

import java.nio.charset.StandardCharsets;
import java.security.MessageDigest;
import java.security.SecureRandom;
import java.util.Base64;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http,
                                           ClientRegistrationRepository clientRegistrationRepository,
                                           AuthenticationSuccessHandler successHandler) throws Exception {

        DefaultOAuth2AuthorizationRequestResolver resolver = new DefaultOAuth2AuthorizationRequestResolver(
                clientRegistrationRepository,
                OAuth2AuthorizationRequestRedirectFilter.DEFAULT_AUTHORIZATION_REQUEST_BASE_URI
        );
        resolver.setAuthorizationRequestCustomizer(this::addPkceParameters);

        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/login/**", "/oauth2/**", "/static/**", "/api/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2Login(oauth2 -> oauth2
                .authorizationEndpoint(auth -> auth.authorizationRequestResolver(resolver))
                .successHandler(successHandler)
            );

        return http.build();
    }

    private void addPkceParameters(OAuth2AuthorizationRequest.Builder builder) {
        try {
            SecureRandom secureRandom = new SecureRandom();
            byte[] bytes = new byte[96];
            secureRandom.nextBytes(bytes);
            String codeVerifier = Base64.getUrlEncoder().withoutPadding().encodeToString(bytes);

            byte[] digest = MessageDigest.getInstance("SHA-256")
                    .digest(codeVerifier.getBytes(StandardCharsets.US_ASCII));
            String codeChallenge = Base64.getUrlEncoder().withoutPadding().encodeToString(digest);

            builder.attributes(attrs -> attrs.put(PkceParameterNames.CODE_VERIFIER, codeVerifier));
            builder.additionalParameters(params -> {
                params.put(PkceParameterNames.CODE_CHALLENGE, codeChallenge);
                params.put(PkceParameterNames.CODE_CHALLENGE_METHOD, "S256");
            });
        } catch (Exception e) {
            throw new RuntimeException("Error calculating PKCE challenge", e);
        }
    }
}
```
