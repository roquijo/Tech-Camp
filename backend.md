# ⚙️ Parte 2 — Backend con Spring Boot & Keycloak

**Objetivo:**  
Construir un backend funcional con Spring Boot, conectar a PostgreSQL, proteger los endpoints con Keycloak y documentar la API con Swagger (springdoc).

---

## 1. Dependencias necesarias (al crear el proyecto)

Al generar el proyecto ([Spring Initializr](https://start.spring.io)) selecciona al menos:

- Spring Web
- Spring Data JPA
- PostgreSQL Driver
- Spring Boot DevTools
- Lombok
- Spring Security NO SELECCIONAR
- OAuth2 Resource Server (`spring-boot-starter-oauth2-resource-server`) NO SELECCIONAR
- springdoc OpenAPI UI (`springdoc-openapi-starter-webmvc-ui` o `springdoc-openapi-ui`) NO APARECE

```
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>3.0.0-M1</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
 ```

> Nota: añade `spring-boot-starter-oauth2-resource-server` y `org.springdoc:springdoc-openapi-starter-webmvc-ui` manualmente despues de la explicacion.

---

## 2. Estructura recomendada (sin código)
Organiza el proyecto en paquetes por responsabilidades:

```
controller/   → endpoints REST
service/      → lógica de negocio
repository/   → interfaces JPA
model/        → entidades / DTOs
config/       → configuraciones (CORS, seguridad, swagger)
resources/
  └─ application.yml
```

---

## 3. `application.yml` (ejemplo mínimo de configuración)

Guarda este bloque en `src/main/resources/application.yml` y ajústalo a tus credenciales / puertos:

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/techcamp
    username: postgres
    password: postgres_password
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: update   # en producción usar validate o none
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect

  jackson:
    serialization:
      indent_output: true

logging:
  level:
    root: INFO
    org.hibernate.SQL: DEBUG

# Configuración de Spring Security como Resource Server (Keycloak)
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: http://localhost:8080/realms/techcamp-realm

# Springdoc (Swagger) - configuración básica
springdoc:
  api-docs:
    path: /v3/api-docs
  swagger-ui:
    enabled: true
    path: /swagger-ui.html
    oauth:
      clientId: spring-api
      clientSecret: YOUR_CLIENT_SECRET_IF_APPLICABLE
      appName: "TechCamp API"
```

> Ajusta `issuer-uri` / `jwk-set-uri` según el host y puerto donde corra Keycloak en tu entorno.

---

## 4. Configuración de Keycloak (resumen de pasos)

1. **Levantar Keycloak (modo dev)**:
   ```bash
   docker run -d --name keycloak      -p 8080:8080      -e KEYCLOAK_ADMIN=admin      -e KEYCLOAK_ADMIN_PASSWORD=admin123      quay.io/keycloak/keycloak:26.0.2 start-dev
   ```

2. **Crear Realm**: `techcamp-realm` (o el nombre que prefieras).

3. **Crear Client**:
   - Client ID: `spring-api` (ejemplo)
   - Access Type: `confidential`
   - Client authentication: **ON**
   - Valid Redirect URIs: `http://localhost:8080/*` o `http://localhost:4200/*`
   - Habilitar **Service Accounts** si vas a usar `grant_type=client_credentials`.

4. **Obtener Client Secret**:
   - Clients → [spring-api] → Credentials → copiar `Client secret`.

5. **Ver endpoints OIDC**:
   - Realm Settings → Endpoints → OpenID Endpoint Configuration → verás `token_endpoint`, `authorization_endpoint`, `jwks_uri`, etc.

---

## 5. Qué permisos abrir en Security para Swagger

Permite acceso público a:
- `/v3/api-docs/**`
- `/swagger-ui/**`
- `/swagger-ui.html`
- `/webjars/**`

---

## 6. Ejemplo de flujo para obtener token (curl / Postman)

### Token con `client_credentials` (servicio → servicio)
```bash
curl -X POST "http://localhost:8080/realms/techcamp-realm/protocol/openid-connect/token"  -H "Content-Type: application/x-www-form-urlencoded"  -d "grant_type=client_credentials"  -d "client_id=spring-api"  -d "client_secret=<TU_CLIENT_SECRET>"
```

### Usar token para llamar a API protegida
```bash
curl -H "Authorization: Bearer <ACCESS_TOKEN>"   http://localhost:8080/api/productos
```

---

## 7. CORS (mención breve)

- Si tu frontend corre en `http://localhost:4200`, habilita CORS en el backend.
- Recomendación: permitir solo orígenes específicos (no `*`) en desarrollo.

---

## 8. Errores comunes

- `invalid_client` → client_id o client_secret incorrectos.
- `unauthorized_client` → Service account no habilitado.
- `invalid_token` → issuer mal configurado.
- Swagger vacío → permisos no abiertos para `/v3/api-docs`.

---

## 9. Checklist final antes de la demo

- [ ] Keycloak corriendo y realm creado.
- [ ] Client (confidential) creado y secret copiado.
- [ ] PostgreSQL corriendo y base de datos creada.
- [ ] `application.yml` correcto.
- [ ] Dependencias incluidas (resource-server, springdoc).
- [ ] Swagger en `http://localhost:8080/swagger-ui.html`.
- [ ] Token probado con Postman o curl.

---

## 10. Recursos útiles

- [Spring Initializr](https://start.spring.io)
- [Keycloak Docs](https://www.keycloak.org/documentation)
- [Spring Security Resource Server](https://spring.io/guides/tutorials/spring-boot-oauth2/)
- [Springdoc OpenAPI](https://springdoc.org)

---

**Resultado esperado:**  
Backend corriendo en `http://localhost:8080`, documentado por Swagger, conectado a PostgreSQL y protegido por Keycloak.
