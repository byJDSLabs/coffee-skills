# OWASP Top 10 — 2025 (Resumen condensado)

Fuente oficial: https://owasp.org/Top10/2025/
Licencia OWASP: CC-BY-SA-4.0. Este archivo es un resumen de referencia para auditoría.

Las 10 categorías de riesgo con sus CWEs más relevantes. El orden indica prevalencia + impacto real observado en la industria.

| # | Categoría | Descripción corta | CWEs clave |
|---|---|---|---|
| A01 | **Broken Access Control** | El usuario actúa fuera de sus permisos: accede a datos/acciones de otros roles. Incluye IDOR, missing authorization, path traversal y (desde 2025) SSRF. | CWE-284, CWE-862, CWE-863, CWE-352 (CSRF), CWE-22, CWE-918 (SSRF) |
| A02 | **Security Misconfiguration** | Configuración insegura: credenciales por defecto, features innecesarias habilitadas, errores verbosos, headers de seguridad ausentes, CORS abierto. | CWE-16, CWE-209, CWE-1004, CWE-1027 |
| A03 | **Software Supply Chain Failures** | Dependencias vulnerables, build systems comprometidos, distribución insegura, falta de verificación de integridad. Evolución de "Vulnerable and Outdated Components" (2021). | CWE-1357, CWE-829, CWE-345, CWE-1104, CWE-937 |
| A04 | **Cryptographic Failures** | Criptografía débil o mal aplicada: transmisión en claro, algoritmos obsoletos (MD5/SHA1/DES), hashes sin salt, claves hardcodeadas. | CWE-261, CWE-319, CWE-327, CWE-328, CWE-311, CWE-329 |
| A05 | **Injection** | Input no confiable ejecutado como código: SQL, NoSQL, OS command, LDAP, XPath, XSS. | CWE-79 (XSS), CWE-89 (SQLi), CWE-73, CWE-77, CWE-78, CWE-94, CWE-95 |
| A06 | **Insecure Design** | Fallas de diseño/arquitectura, no de implementación: falta de rate limiting, trust en input del cliente, flujos de negocio abusables. | CWE-1188, CWE-276, CWE-400, CWE-799 |
| A07 | **Authentication Failures** | Autenticación débil: credenciales débiles, falta de MFA, sesiones no revocadas, respuesta a forgot-password abusable, fallback inseguro. | CWE-287, CWE-306, CWE-307, CWE-308, CWE-521, CWE-640 |
| A08 | **Software and Data Integrity Failures** | Código/datos sin verificación de integridad: deserialización insegura, firmas ausentes, confianza en componentes sin validar. | CWE-502, CWE-345, CWE-494 |
| A09 | **Logging and Alerting Failures** | No hay logs útiles ni alertas; los ataques pasan desapercibidos. Logs con datos sensibles sin redactar. | CWE-117, CWE-223, CWE-532, CWE-778 |
| A10 | **Mishandling of Exceptional Conditions** | Manejo incorrecto de errores/excepciones: sistemas que fallan-open, stack traces expuestos, estados inseguros ante inputs anómalos. Absorbe SSRF (2025). | CWE-209, CWE-248, CWE-390, CWE-404, CWE-459, CWE-600 |

## Notas de evolución 2025 vs 2021

- A01 ahora absorbe SSRF (antes A10:2021).
- A02 subió del 5º al 2º puesto.
- A03 reemplaza "Vulnerable and Outdated Components" y amplía el alcance a toda la cadena de suministro.
- A10 es nueva (antes difuminada en "poor code quality").
- A09 pasó de "Logging and Monitoring" a "Logging and Alerting" (énfasis en alertas).

## Cómo usar esta tabla en la auditoría

1. Para cada endpoint/módulo del sistema, pasar mentalmente las 10 filas y marcar las aplicables.
2. Cada categoría tiene tests concretos en `wstg-methodology.md` (IDs WSTG) y patrones de código en `SKILL.md` Fase 2.
3. Los hallazgos se clasifican con la CWE de causa raíz (ver `cwe-top25.md`) y se puntúan con CVSS (`cvss-scoring.md`).
