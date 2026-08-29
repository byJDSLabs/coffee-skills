# OWASP WSTG v4.2 — Metodología de Pruebas (Resumen)

Fuente oficial: https://owasp.org/www-project-web-security-testing-guide/
El WSTG es la metodología "cómo probar". Cada prueba tiene un ID estable `WSTG-CATEGORÍA-NN`. Se usa para referenciar hallazgos de forma auditable y para saber exactamente qué probar en cada categoría.

## Las 12 categorías

| Código | Categoría | Tests destacados | Uso en auditoría |
|---|---|---|---|
| INFO | Information Gathering | WSTG-INFO-01 al 09: huellas de la app, servidor, tecnologías, mapeo de endpoints | Fase 1 (reconocimiento) |
| CONF | Configuration and Deployment | WSTG-CONF-01 al 12: headers de seguridad, métodos HTTP, CORS, stack traces, admin interfaces | Fase 4.1 |
| IDNT | Identity Management | WSTG-IDNT-01 al 05: registro, roles, account enumeration, default accounts | Fase 4.2 |
| ATHN | Authentication | WSTG-ATHN-01 al 10: fuerza bruta, lockout, forgot password, weak passwords | Fase 4.2 |
| ATHZ | Authorization | WSTG-ATHZ-01 al 04: path traversal, IDOR (IDOR = ATHZ-02), privilege escalation | Fase 4.3 |
| SESS | Session Management | WSTG-SESS-01 al 09: cookies, session fixation, logout, CSRF | Fase 4.2/4.3 |
| INPV | Input Validation | WSTG-INPV-01 al 17: XSS reflejado/almacenado/DOM, SQLi, command injection, XXE, HTTP smuggling | Fase 4.4 |
| CRYP | Cryptography | WSTG-CRYP-01 al 04: TLS débil, datos sensibles en claro, algoritmos obsoletos | Fase 2/4.1 |
| BUSL | Business Logic | WSTG-BUSL-01 al 09: límites, flujos abusables, condiciones de carrera | Fase 4.5 |
| CLNT | Client-Side | WSTG-CLNT-01 al 13: DOM XSS, clickjacking, cross-origin, open redirects, local storage sensible | Fase 4 (frontend) |
| ERRP | Error Handling | WSTG-ERRP-01 al 02: códigos de error, stack traces filtrados | Fase 4.1 |
| APIT | API Testing | WSTG-APIT-01 al 02: autenticación API, errores/información filtrada | Fase 4 (si hay API) |

## Pruebas con los IDs más citados en reportes profesionales

- **WSTG-INPV-02** — Stored XSS
- **WSTG-INPV-01** — Reflected XSS
- **WSTG-INPV-05** — SQL Injection
- **WSTG-INPV-13** — Command Injection (aproximado; ver índice oficial)
- **WSTG-ATHZ-02** — IDOR / testing for direct object references
- **WSTG-ATHN-03** — Weak Lockout Mechanism
- **WSTG-SESS-02** — Cookie Attributes
- **WSTG-SESS-05** — CSRF
- **WSTG-CRYP-01** — Weak TLS/SSL transport layer
- **WSTG-CONF-01** — HTTP Security Headers
- **WSTG-CONF-07** — HTTP Methods
- **WSTG-CLNT-08** — Clickjacking
- **WSTG-BUSL-03** — Integrity / business logic
- **WSTG-ERRP-02** — Stack traces / error page information

> Nota: los números exactos varían entre versiones. Antes de citar un ID en un reporte, verificar el índice oficial de la versión usada.

## Cómo usar en la auditoría

1. En Fase 1, mapear qué categorías aplican al sistema.
2. En Fases 2-4, ejecutar los tests de cada categoría (estático para código, dinámico para la app corriendo).
3. En el reporte, cada hallazgo se referencia con su ID WSTG + categoría OWASP + CWE.

## Regla de oro

El WSTG cubre pruebas técnicas. Los escáneres automatizados cubren una fracción (sobre todo INPV); las categorías **ATHZ, BUSL, IDNT y CLNT** requieren prueba manual y son donde se encuentran los hallazgos más graves (BOLA, IDOR, lógica de negocio).
