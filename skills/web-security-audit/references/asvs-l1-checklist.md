# OWASP ASVS v4 — Checklist condensado Nivel 1

Fuente oficial: https://owasp.org/www-project-application-security-verification-standard/
ASVS define "cómo se ve una app verificada como segura" en 3 niveles. **Nivel 1** es el mínimo para apps con datos de bajo riesgo y es el punto de partida de esta skill. Cada verificación puede confirmarse con tests estáticos y/o dinámicos.

Los IDs usados aquí son de ASVS v4.0.x (formato V{n}.{m}.{s}).

## V1 — Arquitectura, Diseño y Threat Modeling (Nivel 1)

- [ ] V1.1.1/1.2.1 — Los componentes de la app están definidos y el stack documentado.
- [ ] V1.5.1 — El diseño identifica trust boundaries y zonas de datos sensibles.
- [ ] V1.12.1 — Los secretos (API keys, passwords) no están en código ni configs versionadas.

## V2 — Authentication

- [ ] V2.1.1 — No hay credenciales por defecto en la app.
- [ ] V2.1.2 — El registro de cuentas permite activación segura.
- [ ] V2.2.1 — Anti-automation/anti-bruteforce en login (rate limiting o similar).
- [ ] V2.3.1 — Cambio de contraseña requiere la contraseña actual.
- [ ] V2.4.1/2.4.5 — Forgot password usa tokens seguros de un solo uso, no adivinables.
- [ ] V2.5.1 — La verificación de contraseñas compara hashes, no texto plano.
- [ ] V2.8.1 — La política de contraseñas es razonable (mínimo de longitud, no obvia).
- [ ] V2.8.3 — MFA requerido para funciones sensibles (si aplica a la app).

## V3 — Session Management

- [ ] V3.1.1 — La cookie de sesión usa el atributo `Secure`.
- [ ] V3.1.2 — La cookie usa `HttpOnly`.
- [ ] V3.1.3 — La cookie usa `SameSite` (Strict o Lax).
- [ ] V3.1.4 — `__Host-`/`__Secure-` prefix cuando corresponde.
- [ ] V3.2.1 — El logout invalida completamente la sesión en servidor.
- [ ] V3.4.1 — El token de sesión es aleatorio (alta entropía).
- [ ] V3.4.2 — La sesión se genera tras el login (no antes).
- [ ] V3.6.1 — La sesión expira tras inactividad.

## V4 — Access Control

- [ ] V4.1.1 — Hay reglas de autorización por rol/permiso en servidor.
- [ ] V4.1.2 — Deny-by-default en endpoints nuevos.
- [ ] V4.2.1 — Los accesos a objetos validan propiedad (previene BOLA/IDOR).
- [ ] V4.3.1 — Las funciones admin requieren rol admin verificado en servidor.
- [ ] V4.3.3 — El frontend no es la única capa de autorización.

## V5 — Validación, Sanitización y Encoding

- [ ] V5.1.1 — Todo input se valida en servidor (tipo, formato, longitud, rango).
- [ ] V5.2.2 — La salida se encodea según contexto (HTML, URL, JS, CSS).
- [ ] V5.2.3 — No se usa concatenación de strings para construir HTML con datos no confiables.
- [ ] V5.3.3 — No se ejecuta código generado dinámicamente (eval) con datos del usuario.
- [ ] V5.3.8 — El uso de librerías de template escapa por defecto.
- [ ] V5.5.1 — Las queries de BD usan parámetros/prepared statements (previene SQLi).
- [ ] V5.5.4 — Los comandos del SO no se construyen con input del usuario (o se sanitizan).
- [ ] V5.6.1 — No se deserializan datos no confiables.

## V6 — Cryptografía Almacenada

- [ ] V6.1.2 — Las contraseñas se hashean con algoritmo adaptativo (bcrypt/argon2/scrypt) con salt.
- [ ] V6.2.2 — No se usan algoritmos de cifrado débiles (DES, RC4, MD5 para firmas).
- [ ] V6.3.2 — El modo de operación de bloques no es ECB.
- [ ] V6.4.1 — El padding/oráculos no permiten descubrir texto plano (no usar CBC con relleno adivinable).

## V7 — Manejo de Errores y Logging

- [ ] V7.1.1 — Las respuestas de error no revelan stack traces, rutas internas ni detalles de BD.
- [ ] V7.2.1 — El código no loguea datos sensibles (contraseñas, tokens, tarjetas).
- [ ] V7.3.1 — Fallos de auth, cambios de permisos y errores se registran con contexto.

## V8 — Protección de Datos

- [ ] V8.1.2 — Los datos sensibles se identifican y protegen según su clasificación.
- [ ] V8.1.6 — No se colocan datos sensibles en localStorage/sessionStorage (solo session tokens con resguardos).
- [ ] V8.2.1 — Los datos sensibles se enmascaran según corresponda.
- [ ] V8.3.1 — Los datos sensibles en cache se controlan (headers de caché).

## V9 — Comunicaciones

- [ ] V9.1.2 — TLS 1.2+ en toda transmisión de datos sensibles.
- [ ] V9.1.3 — No se envía datos sensibles por canales en claro (HTTP).
- [ ] V9.2.1 — Certificados válidos, config de TLS actualizada.

## V11 — Lógica de Negocio

- [ ] V11.1.1 — No se confía en el cliente para decisiones de negocio críticas.
- [ ] V11.1.2 — Los montos/importes se validan en servidor (no solo en el frontend).
- [ ] V11.1.4 — Límites de cantidad/uso aplicados en servidor.
- [ ] V11.1.7 — Los flujos de negocio no son abusables (repetición, reorden de pasos).

## V12 — Archivos y Recursos

- [ ] V12.1.1 — Las subidas de archivos validan tipo, extensión y contenido real.
- [ ] V12.1.2 — Los archivos subidos no son ejecutables en el servidor.
- [ ] V12.3.1 — Los archivos descargados no permiten path traversal.
- [ ] V12.4.1 — Los archivos subidos no se sirven desde el mismo dominio del app si son no confiables.

## V13 — API y Servicios Web

- [ ] V13.1.3 — Las APIs validan la estructura del JSON/XML recibido.
- [ ] V13.2.1 — Los endpoints de API tienen autenticación cuando corresponde.
- [ ] V13.2.3 — Se rechazan datos malformados sin revelar detalles internos.

## V14 — Configuración

- [ ] V14.1.1 — No hay componentes deshabilitados... (verificar que features de debug/admin no estén en prod).
- [ ] V14.2.1 — Headers de seguridad presentes (CSP, HSTS, X-Frame-Options, nosniff).
- [ ] V14.2.5 — La política CORS está restringida a orígenes confiables.
- [ ] V14.3.1 — Config de hardening aplicada según guías del framework.
- [ ] V14.4.1 — No hay secretos en configs versionadas ni en variables de entorno expuestas.

## Cómo usar este checklist

1. En **Nivel 2** de auditoría, recorrer estas verificaciones contra el código y la app.
2. En **Nivel 1**, cubrir al menos: V2.1-2.5, V3.1-3.2, V4, V5.1-5.5, V6.1, V7.1, V8.1, V9.1, V14.1-14.2.
3. Cada verificación fallida se convierte en un hallazgo del reporte con su ID ASVS.
