---
name: web-security-audit
description: Auditoría completa de seguridad web ética (ethical hacking) para sistemas propios o con autorización explícita. Activa cuando el usuario pida probar, auditar, pentestear, revisar la seguridad o buscar vulnerabilidades en una aplicación web, API, frontend, backend o proyecto completo, o cuando quiera un reporte de seguridad con remediación. No usar para atacar sistemas ajenos sin autorización.
version: 1.0.0
tags: [security, audit, pentest, ethical-hacking, owasp, devsecops]
---

# Web Security Audit

## Cuándo aplicar esta Skill

Aplica las reglas de este documento de forma obligatoria cuando el usuario solicite:
- Auditar la seguridad de una aplicación web, API, frontend, backend o proyecto completo.
- Probar vulnerabilidades en sus propios desarrollos ("probá mi app", "buscame bugs de seguridad").
- Realizar ethical hacking o penetration testing básico/avanzado sobre un objetivo autorizado.
- Generar un reporte de seguridad con vulnerabilidades y remediación.
- Revisar un sistema antes de publicarlo o antes de un despliegue a producción.

**No aplicar** para asistir en ataques a sistemas que el usuario no posee o sin autorización escrita/explícita.

---

## Propósito

Esta skill guía la realización de una **auditoría web completa y ética**: analizar un sistema en profundidad, encontrar vulnerabilidades reales, verificarlas y entregar un **reporte claro y accionable** para que el usuario (aunque sea principiante) entienda y corrija cada problema.

Combina **análisis estático** (revisión de código fuente), **análisis de dependencias y cadena de suministro** (SCA/DevSecOps) y **pruebas dinámicas** (contra la aplicación corriendo), alineada con estándares reconocidos:
- OWASP Top 10 2025 (marco de categorías de riesgo).
- MITRE CWE Top 25 2025 (causa raíz técnica).
- OWASP WSTG v4.2 (metodología de pruebas, IDs estables).
- OWASP ASVS v4 (verificación por niveles).
- OWASP API Security Top 10 y OWASP Top 10 for LLM 2025 (cuando aplique).
- Guías de auditoría específicas para OAuth 2.0/OIDC, GraphQL, WebSockets, Next.js/SSR y CI/CD.
- CVSS v4.0 y CVSS v3.1 (puntuación de severidad).

---

# Guardarraíles Éticos Obligatorios

**Regla #1 — Autorización primero.** Antes de cualquier prueba, confirmar:
1. El usuario es el dueño del sistema o tiene autorización explícita para probarlo.
2. El objetivo está claramente definido (URL/localhost, dominio, scope).
3. Solo se prueban los hosts dentro del alcance declarado.

Si el objetivo parece ajeno, público, de terceros o no está claro quién lo opera: **detener y preguntar**. Nunca asumir autorización.

**Regla #2 — No destructivo.** Prohibido:
- Denegación de servicio (DoS/DDoS), saturación de recursos, fuerza bruta agresiva.
- Borrar o modificar datos, enviar spam, provocar bloqueos de cuentas.
- Pruebas que impacten otros usuarios del sistema.

**Regla #3 — PoC mínimo y confirmado.** Los proofs of concept se ejecutan SOLO con confirmación explícita del usuario, con el payload mínimo necesario y preferentemente en entorno de desarrollo/staging (no producción).

**Regla #4 — Sin exfiltración.** No extraer, copiar ni publicar datos reales de clientes o usuarios. Usar datos de prueba (ej. `alice@test.local`).

**Regla #5 — Reporte responsable.** Los payloads en el reporte se muestran a nivel conceptual o truncados, suficientes para reproducir la vulnerabilidad pero no para abusar de ella. No publicar hallazgos fuera del proyecto.

**Regla #6 — Cambio de contraseñas no permitido.** No cambiar credenciales del sistema bajo prueba.

---

# Flujo de Trabajo Obligatorio (7 Fases)

Seguir SIEMPRE este orden. No saltar fases. Trabajar de lo general a lo específico.

## Fase 0 — Autorización y Alcance

- Identificar el stack y tipo de sistema (consulta `references/wstg-methodology.md` para categorías relevantes).
- Definir con el usuario:
  - **Objetivo**: ruta del proyecto, URL local o remota, dominios.
  - **Cuentas de prueba**: qué roles existen (anónimo, usuario, admin) y credenciales de prueba.
  - **Nivel de auditoría**: Nivel 1 (rápido) o Nivel 2 (completo) — ver sección "Niveles".
  - **Restricciones**: qué zonas NO tocar, horarios, producción sí/no.
- Confirmar autorización (Regla #1). Sin confirmación: no continuar.

## Fase 1 — Reconocimiento (análisis de superficie)

Objetivo: entender el sistema antes de atacarlo. Sin generar tráfico innecesario.

1. **Identificar stack y arquitectura** leyendo el proyecto:
   - `package.json`, `angular.json`, `pyproject.toml`, `go.mod`, `composer.json`, `Dockerfile`, `.github/workflows/`, etc.
   - Frontend vs backend, SPA (Angular/React/Vue) vs SSR (Next.js App Router, Nuxt, SvelteKit).
2. **Enumerar rutas, endpoints y contratos**:
   - Router del frontend (ej. Angular `app.routes.ts`, React router, Next.js `app/` routes).
   - Rutas del backend (controllers, routers, decoradores, Server Actions con `"use server"`).
   - Endpoints GraphQL (`/graphql`, esquemas `.graphql`) y gateways WebSocket/Socket.io.
   - Archivos OpenAPI/Swagger/Postman si existen.
3. **Mapear mecanismos de seguridad**:
   - Autenticación (sesiones, JWT, OAuth 2.0 / OIDC / SSO, API keys).
   - Autorización/roles y permisos de nivel de objeto.
   - Manejo de sesiones y cookies.
   - Middleware de seguridad (CORS, headers, rate limiting).
4. **Identificar puntos de entrada de datos**:
   - Formularios, parámetros URL, headers, cuerpos JSON/XML.
   - IDs de objetos (para IDOR), subidas de archivos, mutaciones GraphQL, frames WebSocket.
5. **Documentar**: listar endpoints + métodos HTTP/WS + nivel de autenticación requerido.

## Fase 2 — Análisis Estático (SAST + revisión manual)

Buscar vulnerabilidades directamente en el código. Consultar `references/owasp-top10-2025.md` y `references/cwe-top25.md` para la lista completa de categorías y patrones.

1. **Búsqueda de patrones peligrosos** por categoría (usar grep/ripgrep):
   - **XSS (CWE-79)**: `innerHTML`, `[innerHTML]`, `bypassSecurityTrust*`, `dangerouslySetInnerHTML`, `v-html`, `eval(`, `document.write`.
   - **SQLi (CWE-89)**: concatenación en queries, interpolación de strings en SQL/ORM, `raw`, `query(`.
   - **Comando (CWE-78)**: `exec(`, `spawn(`, `child_process`, `os.system`, `shell=True`.
   - **Secrets hardcodeados (CWE-798)**: API keys, tokens, contraseñas en código/config (revisar también historial git).
   - **Crypto débil (CWE-327)**: MD5, SHA1, `Math.random` para seguridad, AES-ECB, cifrados sin salt.
   - **Autenticación débil (CWE-287)**: compare contraseñas sin hash, tokens predecibles, falta de validación JWT o `state` en OAuth.
   - **Mass Assignment (CWE-915)**: `User.create(req.body)`, `Model.update(req.body)` bind directo sin filtro de propiedades.
   - **Prototype Pollution (CWE-1321)**: `__proto__`, `Object.assign` o merge profundo en objetos de usuario sin sanitizar.
   - **SSTI (CWE-1336)**: `ejs.render(`, `nunjucks.render(`, `render_template_string(` mezclado con input de usuario.
   - **Server Actions Inseguros**: funciones exportadas en archivos `"use server"` que omiten autenticación/autorización.
   - **Inseguridad de deserialización (CWE-502)**: `JSON.parse` de datos no confiables, `pickle`, `unserialize`.
   - **Path traversal (CWE-22)**: lectura de archivos con rutas de usuario sin validar.
   - **SSRF (CWE-918)**: URLs que el usuario controla y el servidor consulta.
   - **Insecure design**: falta de rate limiting, trust en input del cliente, lógica de negocio manipulable.
2. **Ejecutar herramientas SAST** si están instaladas (ver `references/tools-windows.md`): gitleaks (secretos), semgrep (patrones), etc. Si no están instaladas, NO bloquear: el análisis manual de patrones es la base.
3. **Análisis de autenticación y sesión**:
   - ¿Las contraseñas se guardan con hash adaptativo (bcrypt/argon2/scrypt)? ¿Tienen salt?
   - ¿Los JWT validan firma y expiración? ¿Algoritmo seguro (`RS256`, no `none`/`HS256` con clave pública)?
   - ¿El flujo OAuth 2.0 exige `state` y `redirect_uri` exacta? (ver `references/oauth-sso-checklist.md`).
   - ¿Las cookies tienen `HttpOnly`, `Secure`, `SameSite`?
4. **Autorización por objeto (IDOR) y GraphQL**:
   - ¿Las rutas y resolvedores GraphQL validan propiedad del recurso? (ver `references/graphql-websockets.md`).
5. **Vectores SSR / Frameworks Modernos**: consultar `references/modern-ssr-vectors.md` para Next.js Server Actions y SSTI.

## Fase 3 — Análisis de Dependencias (SCA)

1. **Auditar dependencias** (herramientas nativas Windows, ver `references/tools-windows.md`):
   - Node: `npm audit` (y `npm audit --omit=dev`).
   - Python: `pip-audit`.
   - Otros: osv-scanner, trivy (binarios Windows).
2. **Auditar CI/CD y Contenedores** (ver `references/cicd-iac-security.md`):
   - Workflows de GitHub Actions (`.github/workflows/`): inyección de scripts en `run:`, acciones no pineadas por hash commit SHA, permisos de `GITHUB_TOKEN`.
   - Dockerfile: ejecución como `root`, secretos en capas `ENV`/`ARG`, falta de `.dockerignore`.
3. **Registrar CVEs y hallazgos de cadena de suministro** con su severidad.
4. **Revisar instalación segura**: `npm ci`, `--ignore-scripts` si es crítico, lockfiles versionados.

## Fase 4 — Pruebas Dinámicas (aplicación corriendo)

Requiere que la app esté levantada. El usuario debe iniciar el servidor y proporcionar la URL (ej. `http://localhost:4200` para frontend, `http://localhost:3000` para API).

Usar `curl.exe` (integrado en Windows), PowerShell `Invoke-WebRequest`, o scripts Node. Consultar `references/tools-windows.md` para los comandos base.

### 4.1 Configuración y hardening
- Enviar un GET a la raíz y a endpoints clave; revisar **headers de seguridad**:
  - `X-Frame-Options` / `frame-ancestors` (clickjacking).
  - `Content-Security-Policy` (mitigación XSS).
  - `Strict-Transport-Security` (HSTS).
  - `X-Content-Type-Options: nosniff`.
  - `Referrer-Policy`.
  - `X-XSS-Protection` (legacy, no depende de él).
- Revisar respuesta a errores: ¿filtran stack traces o rutas internas? (A10 Mishandling of Exceptional Conditions).

### 4.2 Autenticación y sesión
- **Headers/cookies**: inspeccionar cookies de sesión (`Set-Cookie`): ¿`HttpOnly`, `Secure`, `SameSite`?
- **Fuerza bruta**: enviar 5+ intentos de login fallidos y verificar si existe rate limiting o bloqueo.
- **Sesión**: ¿el logout invalida la sesión? ¿La cookie cambia tras login?
- **JWT**: descodificar el token (base64). Verificar: ¿firma validada? ¿`alg:none` aceptado? ¿expiración respetada? ¿clave débil?

### 4.3 Autorización (A01 — el más crítico)
- **IDOR**: con cuenta A, acceder al recurso `{id}` de cuenta B (cambiar el id en la URL/cuerpo). Si responde con datos de B → IDOR.
- **Elevación de roles**: si existe admin, probar que un usuario normal no puede llamar endpoints admin.
- **Métodos HTTP**: probar `PUT/DELETE/OPTIONS` no permitidos, methods override (`X-HTTP-Method-Override`).
- **Path traversal**: probar `/api/../` o `..%2f..%2fetc/passwd` en rutas de archivos.

### 4.4 Inyección (A05)
- **SQLi**: en parámetros, probar payloads NO destructivos que alteren la respuesta:
  - `' OR '1'='1` solo si es seguro confirmar; mejor payloads que provoquen error controlado: `'` y observar mensajes de error SQL.
  - Time-based: `' OR SLEEP(1)--` (descartar si hay rate limiting / no es necesario).
- **NoSQLi**: en JSON: `{"username": {"$ne": null}}`, `{"$gt": ""}`.
- **Command injection**: inputs que alimentan `exec`/shell.
- **XSS**: introducir `"><script>alert(1)</script>` (o payload conceptual) en campos reflejados y verificar si se refleja sin escapar en la respuesta.
- **SSRF**: si hay endpoints que reciben URLs, probar contra `http://127.0.0.1` y observar.

### 4.5 GraphQL y WebSockets
- **GraphQL**: probar introspección expuesta, profundidades masivas (Query Depth DoS), batching de mutaciones (bypass de rate limiting) y autorización en resolvedores anidados. Consultar `references/graphql-websockets.md`.
- **WebSockets**: verificar validación de la cabecera `Origin` en el handshake (CSWSH), expiración de sesión en conexiones persistentes y sanitización de mensajes.

### 4.6 Lógica de negocio y arquitecturas modernas (WSTG-BUSL)
- **OAuth 2.0 / SSO**: probar redirección maliciosa (`redirect_uri`), ausencia de `state` (CSRF), PKCE bypass y coincidencia de emails. Consultar `references/oauth-sso-checklist.md`.
- **Server Actions & SSR**: probar invocación directa de Server Actions sin sesión, Prototype Pollution y SSTI. Consultar `references/modern-ssr-vectors.md`.
- **Lógica de negocio**: revisar flujos de pago, descuentos, montos negativos, límites de cantidad, reutilización de cupones.

### 4.7 Automatización DAST con Nuclei y ZAP
- **Nuclei**: ejecutar escaneo dinámico basado en plantillas comunitarias contra la app levantada (`winget install projectdiscovery.nuclei`). Comandos de referencia en `references/tools-windows.md`.
- **OWASP ZAP (opcional vía MCP)**: si está configurado el add-on MCP, ejecutar spider + active scan y cruzar alertas.

## Fase 5 — Verificación y Descarte de Falsos Positivos

Antes de reportar cada hallazgo:
1. **Confirmar que es real**: reproducir el hallazgo con la técnica mínima necesaria.
2. **Descartar falsos positivos** de herramientas: revisar el código que origina la alerta.
3. **Evaluar explotabilidad real**: ¿requiere admin? ¿solo afecta datos propios del usuario? ¿requiere condiciones irreales?
4. **Clasificar** cada hallazgo confirmado en: Crítico / Alto / Medio / Bajo / Informativo usando **CVSS v4.0** (estándar principal) o CVSS v3.1 (ver `references/cvss-v4-scoring.md` y `references/cvss-scoring.md`).

## Fase 6 — Reporte Final

Generar el archivo de reporte **`security-report.md`** en la raíz del proyecto auditado, usando la plantilla de `references/report-template.md`. El reporte DEBE incluir:
1. Resumen ejecutivo (riesgo general, hallazgos críticos en 3 líneas).
2. Alcance, autorización y metodología (con IDs WSTG/ASVS/CWE).
3. Tabla de hallazgos priorizada (severidad CVSS, categoría OWASP, CWE, ubicación, evidencia, remediación).
4. Detalle por hallazgo con sección educativa: **Qué es** · **Cómo se explota** · **Evidencia** · **Cómo corregirlo** · **Verificación de la corrección**.
5. Plan de remediación priorizado.
6. Checklist de re-auditoría (para verificar después de corregir).

---

# Niveles de Auditoría

- **Nivel 1 — Rápido (por defecto)**: OWASP Top 10 2025, estático (patrones + gitleaks + npm audit) y dinámico ligero (headers, auth, IDOR básico, inyección básica). Para una primera pasada.
- **Nivel 2 — Completo**: Nivel 1 + ASVS Nivel 1/2 completo (V1–V14), pruebas WSTG profundas por categoría, lógica de negocio, JWT avanzado, SSRF, deserialización. Requiere más tiempo y cuentas de prueba de varios roles.

El usuario elige el nivel en Fase 0. Por defecto iniciar en Nivel 1 y proponer subir a Nivel 2 si el usuario quiere profundizar.

---

# Herramientas (Windows Nativo)

Referencia completa con comandos de instalación: `references/tools-windows.md`.

| Categoría | Herramienta | Instalación | Uso |
|---|---|---|---|
| HTTP | `curl.exe` | Integrado en Windows 10+ | Pruebas dinámicas |
| HTTP | PowerShell `Invoke-WebRequest` | Integrado | Pruebas dinámicas |
| Secretos | gitleaks | `winget install Gitleaks.Gitleaks` | Escanear historial git |
| SAST | semgrep | `pip install semgrep` (o Docker) | Patrones de código |
| SCA | `npm audit` | Con Node.js | Vulnerabilidades npm |
| SCA | osv-scanner | binario GitHub Releases | Multi-lenguaje |
| SCA/Contenedores | trivy | binario GitHub Releases | Dependencias + config |
| DAST | nuclei | `winget install projectdiscovery.nuclei` | Escaneo dinámico con plantillas |
| DAST (opcional) | OWASP ZAP + MCP add-on | instalador Windows | Spider + active scan |

---

# Reglas Obligatorias

- Nunca probar sistemas ajenos o fuera de alcance.
- Nunca ejecutar pruebas destructivas (DoS, borrado de datos, cambio de contraseñas).
- Nunca ejecutar PoC sin confirmación explícita del usuario.
- Nunca extraer ni publicar datos reales de usuarios.
- Nunca asumir que una herramienta detecta todo: el análisis manual del código es obligatorio.
- Nunca reportar un hallazgo sin verificar (Fase 5).
- Siempre explicar los hallazgos en términos que un principiante pueda entender y corregir.
- Siempre entregar el reporte en `security-report.md` salvo que el usuario pida otro formato.

---

# Modo Aprendizaje

Esta skill también funciona como **guía de estudio** si el usuario lo pide ("explícame", "enséñame"):
- Explicar cada vulnerabilidad encontrada con analogías del mundo real.
- Mostrar el payload y por qué funciona (nivel conceptual).
- Recomendar laboratorios de práctica legales para cada categoría:
  - **OWASP Juice Shop** y **DVWA** (apps vulnerables para practicar).
  - **PortSwigger Web Security Academy** (labs gratuitos por categoría).
  - **VulnHub / TryHackMe / Hack The Box** (máquinas de práctica, solo en entornos dedicados).

---

# Referencias de la Skill

Los archivos en `references/` son resúmenes condensados para consulta rápida durante la auditoría:
- `owasp-top10-2025.md` — Categorías OWASP Top 10 2025 con CWEs mapeadas.
- `cwe-top25.md` — Las 25 debilidades más peligrosas (MITRE 2025).
- `api-top10.md` — OWASP API Security Top 10.
- `llm-top10.md` — OWASP Top 10 for LLM Applications 2025.
- `oauth-sso-checklist.md` — Guía de auditoría OAuth 2.0, OIDC y SSO.
- `graphql-websockets.md` — Auditoría de APIs GraphQL y WebSockets.
- `modern-ssr-vectors.md` — Vectores Next.js Server Actions, Prototype Pollution y SSTI.
- `cicd-iac-security.md` — Auditoría de CI/CD (GitHub Actions) y Docker/IaC.
- `wstg-methodology.md` — Categorías WSTG y pruebas clave.
- `asvs-l1-checklist.md` — Checklist condensado ASVS Nivel 1.
- `cvss-v4-scoring.md` — Guía rápida CVSS v4.0 (FIRST).
- `cvss-scoring.md` — Guía rápida CVSS v3.1 (legacy).
- `tools-windows.md` — Herramientas y comandos para Windows.
- `report-template.md` — Plantilla obligatoria del reporte.

Fuentes oficiales (consultar si se necesita detalle):
- OWASP Top 10 2025: https://owasp.org/Top10/2025/
- OWASP WSTG: https://owasp.org/www-project-web-security-testing-guide/
- OWASP ASVS: https://owasp.org/www-project-application-security-verification-standard/
- MITRE CWE Top 25: https://cwe.mitre.org/top25/
- CVSS: https://www.first.org/cvss/
