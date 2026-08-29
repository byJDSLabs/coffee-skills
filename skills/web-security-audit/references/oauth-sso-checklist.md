# Checklist de Auditoría: OAuth 2.0, OIDC y SSO

Esta guía complementa la auditoría de seguridad cuando la aplicación web o API utiliza flujos de autenticación o autorización basados en **OAuth 2.0**, **OpenID Connect (OIDC)** o **Single Sign-On (SSO)** (Auth0, Okta, Google, GitHub, Azure AD, Keycloak, etc.).

---

## 1. Vectores de Ataque Críticos en OAuth 2.0 / OIDC

### 1.1 Manipulation del Parameter `redirect_uri` (Account Takeover)
- **Riesgo**: Si el servidor de autorización acepta URLs de redirección no estrictamente validadas, un atacante puede robar el `code` de autorización o el `access_token`.
- **Pruebas**:
  - Probar cambiar `redirect_uri=https://client.com/callback` por `https://attacker.com/callback`.
  - Probar bypasses de subdominio: `https://client.com.attacker.com/callback` o `https://attacker-client.com/callback`.
  - Probar Directory Traversal en la URI: `https://client.com/callback/../../attacker`.
  - Probar inyección de caracteres: `https://client.com/callback%0d%0aHost:%20attacker.com`.
  - Probar esquemas de URI personalizados en apps móviles/desktop (`customapp://...`).
- **Verificación de Remediación**: El servidor DEBE validar la `redirect_uri` contra una lista blanca exacta (match exacto de string, no expresiones regulares laxas).

---

### 1.2 Ausencia o Manipulación del Parámetro `state` (OAuth CSRF / Account Linking Attack)
- **Riesgo**: El parámetro `state` vincula la sesión del usuario en el cliente con la solicitud de autorización OAuth. Sin un `state` impredecible e inequívocamente asociado a la cookie de sesión del usuario, un atacante puede vincular su propia cuenta del proveedor (ej. su cuenta de Google) a la cuenta de la víctima en la aplicación objetivo.
- **Pruebas**:
  - Interceptar el flujo de login OAuth y eliminar el parámetro `state`.
  - Probar enviar un valor fijo o estático en `state` (ej. `state=12345`).
  - Probar usar el valor de `state` generado para el Usuario A en la sesión del Usuario B.
- **Verificación de Remediación**: El cliente DEBE generar un token criptográficamente azaroso por sesión (asociado a la cookie `SameSite=Lax` / `Strict`), enviarlo en `state` y verificar que el valor devuelto sea idéntico antes de procesar el `code`.

---

### 1.3 Pre-Authorization Account Takeover (Email Mismatch / Unverified Emails)
- **Riesgo**: Ocurre cuando un usuario se registra manualmente con `victima@empresa.com` (sin verificar email) y luego un atacante crea una cuenta en el IDP (ej. en un proveedor OAuth secundario) usando el mismo email `victima@empresa.com`. Si la app combina automáticamente las cuentas por correo sin verificar si el email en el IDP fue verificado (`email_verified: true`), el atacante toma control de la cuenta.
- **Pruebas**:
  - Verificar si la aplicación confía ciegamente en el campo `email` devuelto por el `id_token` o endpoint `/userinfo`.
  - Comprobar si la app valida la propiedad `email_verified: true` del claim OIDC.
- **Verificación de Remediación**: Requerir siempre confirmación de email o validar explícitamente `email_verified === true` antes de asociar una cuenta existente.

---

### 1.4 Bypass de PKCE (Proof Key for Code Exchange)
- **Riesgo**: En clientes públicos (SPAs Angular/React/Vue y apps móviles), PKCE (`code_challenge` y `code_verifier`) evita el robo de código de autorización.
- **Pruebas**:
  - Interceptar la petición `/authorize`, eliminar `code_challenge` y `code_challenge_method`.
  - Si el servidor acepta el flujo sin `code_challenge`, el soporte de PKCE es opcional (vulnerable).
  - Probar enviar `code_challenge_method=plain` en lugar de `S256`.
- **Verificación de Remediación**: El servidor de autorización DEBE exigir PKCE con método `S256` para todos los clientes públicos.

---

### 1.5 Vulnerabilidades en la Firma y Validación de `id_token` (JWT)
- **Riesgo**: El cliente decodifica el `id_token` (JWT) para extraer la identidad del usuario. Si la librería o el código cliente no valida la firma correctamente, el atacante puede forjar tokens de identidad.
- **Pruebas**:
  - Cambiar el algoritmo del header JWT a `"alg": "none"` y remover la firma.
  - Cambiar el algoritmo de `RS256` (asimétrico) a `HS256` (simétrico) usando la clave pública RSA del IDP como clave secreta HMAC.
  - Modificar los claims `sub`, `email` o `roles` y reenviar el token.
  - Verificar si el claim `aud` (audience) coincide exactamente con el `client_id` de la app.
  - Verificar si el claim `iss` (issuer) coincide exactamente con el dominio del IDP.
- **Verificación de Remediación**: Usar librerías maduras de validación OIDC (ej. `openid-client`) que verifiquen firma vía JWKS (`/.well-known/jwks.json`), `iss`, `aud` y expiración (`exp`).

---

## 2. Checklist Rápido de Auditoría OAuth / SSO

- [ ] ¿La `redirect_uri` requiere coincidencia exacta (exact string match)?
- [ ] ¿Se valida el parámetro `state` contra CSRF vinculándolo a la sesión actual?
- [ ] ¿Es PKCE obligatorio con `code_challenge_method=S256` para la SPA/App móvil?
- [ ] ¿El backend valida los claims `iss`, `aud`, `exp` y la firma JWKS de cada token de IDP?
- [ ] ¿Se verifica `email_verified: true` antes de vincular o crear usuarios vía SSO?
- [ ] ¿Los secret de cliente (`client_secret`) permanecen 100% en el backend y NUNCA se exponen en el bundle frontend o JavaScript?
- [ ] ¿El token de acceso (`access_token`) se almacena en cookies `HttpOnly; Secure; SameSite=Lax` en lugar de `localStorage`?
