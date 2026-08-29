# MITRE CWE Top 25 — 2025 (Resumen condensado)

Fuente oficial: https://cwe.mitre.org/top25/
Basado en análisis de 39,080 registros CVE (jun 2024 – jun 2025). El ranking cambia anualmente.

## Top 10 confirmado (2025)

| Pos | CWE | Nombre |
|---|---|---|
| 1 | CWE-79 | Cross-site Scripting (XSS) |
| 2 | CWE-89 | SQL Injection |
| 3 | CWE-352 | Cross-Site Request Forgery (CSRF) |
| 4 | CWE-862 | Missing Authorization |
| 5 | CWE-787 | Out-of-bounds Write |
| 6 | CWE-22 | Improper Limitation of a Pathname to a Restricted Directory (Path Traversal) |
| 7 | CWE-416 | Use After Free |
| 8 | CWE-125 | Out-of-bounds Read |
| 9 | CWE-78 | OS Command Injection |
| 10 | CWE-94 | Code Injection |

## Entradas nuevas en 2025

| Pos | CWE | Nombre |
|---|---|---|
| 11 | CWE-120 | Buffer Copy without Checking Size of Input (Classic Buffer Overflow) |
| 14 | CWE-121 | Stack-based Buffer Overflow |
| 16 | CWE-122 | Heap-based Buffer Overflow |
| 19 | CWE-284 | Improper Access Control |
| 24 | CWE-639 | Authorization Bypass Through User-Controlled Key |
| 25 | CWE-770 | Allocation of Resources Without Limits or Throttling |

## Otras CWEs habitualmente en el Top 25

Con posición variable entre años (consultar la lista oficial para el orden completo):

- CWE-269 (Improper Privilege Management)
- CWE-306 (Missing Authentication for Critical Function)
- CWE-400 (Uncontrolled Resource Consumption)
- CWE-476 (NULL Pointer Dereference)
- CWE-502 (Deserialization of Untrusted Data)
- CWE-611 (Improper Restriction of XML External Entity Reference / XXE)
- CWE-798 (Use of Hard-coded Credentials)
- CWE-190 (Integer Overflow or Wraparound)

## Cómo usar esta lista en la auditoría

1. **Revisión de código (Fase 2)**: buscar patrones que causen estas debilidades (ver SKILL.md). Son las causas raíz más explotadas.
2. **Clasificación**: cada hallazgo del reporte se etiqueta con su CWE (ej. un XSS → CWE-79).
3. **Priorización**: un hallazgo que mapea a una CWE del Top 25 tiene prioridad de corrección alta.
4. No confundir CWE (tipo de debilidad) con CVE (vulnerabilidad concreta en un producto).
