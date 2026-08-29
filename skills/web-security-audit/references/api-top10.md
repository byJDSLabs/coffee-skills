# OWASP API Security Top 10 (2023) — Resumen

Fuente oficial: https://owasp.org/API-Security/
Versión vigente: 2023.

| # | Categoría | Descripción | Cómo probarla |
|---|---|---|---|
| API1 | **Broken Object Level Authorization (BOLA)** | El cliente accede a objetos que no le pertenecen (cambiar `{id}` en la URL/cuerpo). Equivale a IDOR. | Con dos cuentas A/B: usar el id del objeto de B con la sesión de A. |
| API2 | **Broken Authentication** | Mecanismos de auth débiles: credenciales predecibles, falta de rate limiting, tokens de sesión mal generados, cambio de contraseña abusable. | Probar fuerza bruta, reutilización de tokens, JWT inválido, endpoints olvidados sin auth. |
| API3 | **Broken Object Property Level Authorization (BOPLA)** | El cliente puede leer/escribir propiedades de objetos que no debería (mass assignment, enums de campos). | Enviar campos extra en el body (ej. `"isAdmin": true`), ver si se aplican; ver si la respuesta expone campos sensibles. |
| API4 | **Unrestricted Resource Consumption** | Sin límites: rate limiting ausente, bodies enormes, query complexity. | Enviar paginación enorme, payloads grandes, muchos requests; observar uso de recursos. |
| API5 | **Broken Function Level Authorization** | Un usuario normal puede llamar funciones admin (endpoints no protegidos). | Enumerar endpoints admin (fuzzing de rutas) y probarlos con rol usuario. |
| API6 | **Unrestricted Access to Sensitive Business Flows** | Flujos de negocio valiosos sin protección (ej. compra de entradas, scraping de precios, registro masivo). | Identificar flujos críticos y probar si hay controles de acceso automatizado. |
| API7 | **Server Side Request Forgery (SSRF)** | La API recibe una URL y la consulta sin validar. | Enviar URLs a `http://127.0.0.1`, `http://169.254.169.254` (metadata cloud), `file://`. |
| API8 | **Security Misconfiguration** | Headers faltantes, CORS abierto, métodos HTTP innecesarios, errores verbosos. | Inspeccionar respuestas, probar OPTIONS/CORS con origen malicioso. |
| API9 | **Improper Inventory Management** | APIs/versiones antiguas o de debug expuestas y sin mantener. | Fuzzing de `/api/v1`, `/v2`, `/dev`, `/swagger`, `/graphql`. |
| API10 | **Unsafe Consumption of APIs** | La app consume APIs de terceros de forma insegura (sin validar respuestas, sin verificar integridad). | Revisar el código de integraciones externas. |

## Checklist rápido para APIs REST

- [ ] OpenAPI/Swagger accesible públicamente (debería estar deshabilitado en prod).
- [ ] Autenticación en TODOS los endpoints, incluidos los de salud/debug.
- [ ] Rate limiting en login, registro y endpoints sensibles.
- [ ] Respuestas de error sin stack traces ni detalles internos.
- [ ] CORS restringido a orígenes confiables.
- [ ] Paginación acotada.
- [ ] Sin métodos HTTP innecesarios (deshabilitar `TRACE`, `OPTIONS` de ser posible).
- [ ] Tokens/sesiones con expiración y revocación.
- [ ] Validación de tipos y tamaños de input.

## Cómo usar en la auditoría

- Si el proyecto tiene backend/API, correr esta lista en paralelo al OWASP Top 10 2025.
- Las categorías BOLA/BOPLA/BFLA (API1/3/5) son las más explotadas y las que los escáneres suelen perder: hacerlas manualmente con 2-3 cuentas de prueba.
