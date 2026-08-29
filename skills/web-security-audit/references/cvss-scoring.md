# CVSS v3.1 — Guía rápida de puntuación

Fuente oficial: https://www.first.org/cvss/
CVSS estandariza la severidad de una vulnerabilidad (0.0 – 10.0). Se usa en el reporte para priorizar correcciones.

## Rangos de severidad

| Puntaje | Severidad | Color mental |
|---|---|---|
| 9.0 – 10.0 | **Critical** | Explotación remota simple, impacto total |
| 7.0 – 8.9 | **High** | Explotable, impacto alto |
| 4.0 – 6.9 | **Medium** | Requiere condiciones, impacto medio |
| 0.1 – 3.9 | **Low** | Impacto limitado |
| 0.0 | **None** | Sin impacto |

## Métricas base (las que puntúan el vector)

- **AV — Attack Vector**: N (red/remoto), A (adyacente), L (local), P (físico).
- **AC — Attack Complexity**: L (baja) o H (alta).
- **PR — Privileges Required**: N (ninguno), L (bajo), H (alto).
- **UI — User Interaction**: N (ninguna) o R (requerida).
- **S — Scope**: U (sin cambio) o C (cambio de scope).
- **C — Confidentiality**: N/L/H.
- **I — Integrity**: N/L/H.
- **A — Availability**: N/L/H.

## Cómo puntuar un hallazgo de forma rápida

Para la mayoría de hallazgos de una auditoría web se puede estimar:

| Escenario típico | Vector base | Score |
|---|---|---|
| SQLi remota, sin auth, datos completos | `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H` | 9.8 Critical |
| XSS reflejado que requiere que la víctima haga clic | `AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:L/A:N` | 7.1 High |
| IDOR que expone datos de otro usuario (autenticado) | `AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N` | 6.5 Medium |
| Header de seguridad ausente (ej. CSP) | `AV:N/AC:H/PR:N/UI:R/S:U/C:N/I:N/A:N` | 2.6 Low |
| Info leak en error (stack trace) | `AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N` | 5.3 Medium |

## Recomendación práctica

1. Usar los ejemplos de tabla como base y ajustar según contexto real.
2. Si se quiere precisión total: https://www.first.org/cvss/calculator/3.1
3. El vector debe quedar escrito en el reporte (ej. `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`).

## Reglas del reporte

- Todo hallazgo con severidad **Medium+** debe incluir vector CVSS y score.
- Los **Critical** son los que se atienden primero, aunque no siempre son los más fáciles de explotar: el plan de remediación pondera score × probabilidad de explotación real × valor del dato afectado.
