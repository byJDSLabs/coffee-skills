# Plantilla de Reporte de Auditoría Web

Generar el reporte como `security-report.md` en la raíz del proyecto auditado. Sustituir los campos entre `{{ }}`. Mantener esta estructura y el orden.

```markdown
# Reporte de Auditoría de Seguridad Web

## 1. Resumen Ejecutivo

- **Proyecto:** {{ nombre }}
- **Fecha:** {{ fecha }}
- **Auditor:** {{ nombre del usuario / equipo }}
- **Objetivo auditado:** {{ URL / rutas }}
- **Riesgo general:** {{ Bajo / Medio / Alto / Crítico }} — {{ 1 línea de justificación }}
- **Hallazgos:** {{ n }} totales ({{ n }} Críticos, {{ n }} Altos, {{ n }} Medios, {{ n }} Bajos, {{ n }} Informativos)

Resumen en 3 líneas:
1. {{ hallazgo más crítico }}
2. {{ segundo hallazgo más crítico }}
3. {{ tendencia o fortaleza encontrada }}

## 2. Alcance, Autorización y Metodología

- **Alcance:** {{ hosts/rutas probadas }}
- **Autorización:** {{ confirmado por / contrato / solo sistemas propios }}
- **Entorno:** {{ desarrollo / staging / producción }}
- **Nivel de auditoría:** {{ Nivel 1 / Nivel 2 }}
- **Cuentas de prueba usadas:** {{ roles y credenciales de prueba }}
- **Metodología y referencias:** OWASP Top 10 2025 · OWASP WSTG v4.2 · OWASP ASVS v4 (L1) · MITRE CWE Top 25 · OWASP API Top 10 · OWASP LLM Top 10 · CVSS v3.1
- **Herramientas:** {{ gitleaks, npm audit, semgrep, curl, ZAP... (las que se usaron) }}
- **Restricciones respetadas:** {{ no-DoS, sin datos reales, PoC confirmados... }}

## 3. Tabla de Hallazgos Priorizada

| # | Severidad | Categoría OWASP | CWE | Ubicación | Título | CVSS | Referencia |
|---|-----------|-----------------|-----|-----------|--------|------|------------|
| 1 | Crítico | A01 | CWE-862 | `src/api/orders.ts:42` | IDOR en /api/orders/{id} | 6.5 | WSTG-ATHZ-02 |

## 4. Detalle de Hallazgos

### 4.1 — {{ Título del hallazgo }} — {{ Severidad }}

- **Categoría:** {{ OWASP }} · **CWE:** {{ }} · **Referencia:** {{ WSTG/ASVS }}
- **CVSS:** `CVSS:3.1/...` ({{ score }})
- **Ubicación:** {{ archivo:línea / endpoint }}

**Qué es:**
{{ Explicación para principiante con analogía. }}

**Cómo se explota:**
{{ Pasos de reproducción. Payloads a nivel conceptual/truncado. }}

**Evidencia:**
{{ Request/response relevante o fragmento de código. }}

**Cómo corregirlo:**
{{ Solución concreta. Enlaces a la práctica recomendada. }}

**Verificación de la corrección:**
{{ Test que confirma que quedó resuelto. }}

(repetir por cada hallazgo, ordenado por severidad)

## 5. Plan de Remediación Priorizado

1. **Inmediato (esta semana):** {{ hallazgos críticos y altos }}
2. **Corto plazo (1-2 semanas):** {{ hallazgos medios }}
3. **Medio plazo (1-2 meses):** {{ hallazgos bajos y mejoras de proceso }}

## 6. Checklist de Re-auditoría

- [ ] {{ hallazgo 1 }} — corregido y verificado
- [ ] {{ hallazgo 2 }} — corregido y verificado
- [ ] ...

## 7. Notas para el aprendizaje

- {{ Vulnerabilidades encontradas y qué patrón causó cada una }}
- {{ Recursos recomendados para estudiar (laboratorios) }}

---
*Reporte generado con la skill web-security-audit.*
```

## Reglas del reporte

- Todos los hallazgos reportados deben estar **verificados** (Fase 5).
- Incluir siempre la sección educativa "Qué es / Cómo se explota / Cómo corregirlo".
- No incluir credenciales reales ni datos de usuarios reales.
- Los payloads van truncados o conceptuales.
- Señalar si alguna herramienta no pudo ejecutarse (cobertura limitada).
