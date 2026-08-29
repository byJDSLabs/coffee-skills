# Guía Rápida de Clasificación: CVSS v4.0 (FIRST Standard)

Fuente oficial: https://www.first.org/cvss/v4.0/specification-document
CVSS v4.0 es la última generación del sistema estándar de puntuación de vulnerabilidades de ciberseguridad.

---

## 1. Métricas Base de CVSS v4.0

CVSS v4.0 divide el impacto en dos sistemas diferenciados: **Vulnerable System** (el componente donde reside el fallo) y **Subsequent System** (sistemas posteriores o infraestructura afectada aguas abajo).

### 1.1 Métricas de Explotabilidad (Exploitability)

| Métrica | Valores Posibles | Descripción |
|---|---|---|
| **Attack Vector (AV)** | **Network (N)** / **Adjacent (A)** / **Local (L)** / **Physical (P)** | Por dónde se ejecuta el ataque. En aplicaciones web suele ser `N`. |
| **Attack Complexity (AC)** | **Low (L)** / **High (H)** | Dificultad técnica del ataque (ej. carreras de condición, evasion de ASLR/DEP). |
| **Attack Requirements (AT)** | **None (N)** / **Present (P)** | Si el ataque requiere condiciones de red o configuración previas fuera del control del atacante. |
| **Privileges Required (PR)** | **None (N)** / **Low (L)** / **High (H)** | Nivel de permisos requeridos (anónimo `N`, usuario normal `L`, admin `H`). |
| **User Interaction (UI)** | **None (N)** / **Passive (P)** / **Active (A)** | Requiere acción de la víctima (`N` sin interacción, `P` visitar link/ver imagen, `A` clic explícito/aceptar prompt). |

---

### 1.2 Métricas de Impacto (Impact Metrics)

CVSS v4.0 evalúa la **Confidencialidad (C)**, **Integridad (I)** y **Disponibilidad (A)** por separado para el sistema directamente afectado y para los sistemas secundarios.

| Métrica de Impacto | Valores Posibles | Descripción |
|---|---|---|
| **Vulnerable System Confidentiality (VC)** | **High (H)** / **Low (L)** / **None (N)** | Pérdida de confidencialidad en el componente vulnerable. |
| **Vulnerable System Integrity (VI)** | **High (H)** / **Low (L)** / **None (N)** | Modificación no autorizada de datos en el componente vulnerable. |
| **Vulnerable System Availability (VA)** | **High (H)** / **Low (L)** / **None (N)** | Interrupción del servicio en el componente vulnerable. |
| **Subsequent System Confidentiality (SC)** | **High (H)** / **Low (L)** / **None (N)** | Impacto de confidencialidad en sistemas secundarios/asociados. |
| **Subsequent System Integrity (SI)** | **High (H)** / **Low (L)** / **None (N)** | Impacto de integridad en sistemas secundarios/asociados. |
| **Subsequent System Availability (SA)** | **High (H)** / **Low (L)** / **None (N)** | Impacto de disponibilidad en sistemas secundarios/asociados. |

---

## 2. Escala Cualitativa de Severidad

| Puntuación CVSS v4.0 | Nivel de Severidad | Prioridad de Remedación Sugerida |
|---|---|---|
| **0.0** | **None (Informativo)** | Sin acción requerida / Mejora de hardening opcional |
| **0.1 – 3.9** | **Low (Bajo)** | Mitigar en próximo ciclo de mantenimiento / sprint |
| **4.0 – 6.9** | **Medium (Medio)** | Remediar dentro de los próximos 30 días |
| **7.0 – 8.9** | **High (Alto)** | Remediar dentro de los próximos 7 días |
| **9.0 – 10.0** | **Critical (Crítico)** | Remediar de forma inmediata (< 24 horas / Hotfix) |

---

## 3. Matriz Comparativa: CVSS v3.1 vs CVSS v4.0

| Característica | CVSS v3.1 | CVSS v4.0 |
|---|---|---|
| **Métrica de Alcance** | `Scope (S)` (Unchanged / Changed) | Eliminado. Reemplazado por distinción explícita de `Vulnerable System` vs `Subsequent System` (`VC/VI/VA` y `SC/SI/SA`). |
| **Interacción de Usuario** | `None (N)` / `Required (R)` | Refinado en `None (N)`, `Passive (P)` y `Active (A)`. |
| **Condiciones Previas** | Fusionadas en `Attack Complexity` | Separadas en `Attack Complexity (AC)` y `Attack Requirements (AT)`. |
| **Nomenclatura** | Vector único CVSS v3.1 | CVSS-B (Base), CVSS-BT (Threat), CVSS-BE (Environmental), CVSS-BTE (Combinado). |

---

## 4. Ejemplos de Vectores CVSS v4.0 Comunes

### Ejemplo A: SQL Injection en Endpoint Público de Login
- **Vector**: `CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N`
- **Puntuación**: **9.3 (Crítico)**

### Ejemplo B: Stored XSS en Panel de Comentarios (Requiere Login)
- **Vector**: `CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:P/VC:L/VI:L/VA:N/SC:N/SI:N/SA:N`
- **Puntuación**: **5.1 (Medio)**

### Ejemplo C: Pre-Auth RCE vía Next.js Server Action
- **Vector**: `CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H`
- **Puntuación**: **10.0 (Crítico)**
