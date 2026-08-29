# coffee-skills

Base de conocimiento estructurada de skills para asistentes de IA — flujos de trabajo reutilizables y disciplinados que enseñan a una IA a realizar tareas específicas con consistencia y precisión.

Cada skill es un paquete autónomo de instrucciones, principios, flujos de trabajo y documentos de referencia.

---

## Skills

| Skill | Versión | Descripción |
|---|---|---|
| [`ui-migration`](./skills/ui-migration/) | `1.0.0` | Migración pixel-perfect de interfaces de usuario entre tecnologías preservando la máxima fidelidad visual y funcional. |
| [`web-security-audit`](./skills/web-security-audit/) | `1.0.0` | Auditoría ética de seguridad web (OWASP Top 10 2025, WSTG, ASVS, CVSS) con análisis estático, SCA y pruebas dinámicas, entregando reporte con remediación. |

---

## Estructura del proyecto

```
coffee-skills/
  README.md
  skills/
    <skill-name>/
      SKILL.md              # Definición principal de la skill
      knowledge/            # Documentos de referencia
      references/           # Documentos de referencia (alternativa a knowledge/)
```

---

## Contribuir

Nuevas skills deben seguir la misma estructura:

1. Crear una rama `skill/<skill-name>`
2. Agregar un `SKILL.md` con frontmatter, propósito, flujo de trabajo, reglas y formato de salida
3. Agregar el directorio de referencias (`knowledge/` o `references/`) con documentos de referencia según sea necesario
4. Actualizar este README con la entrada de la skill en la tabla de arriba

---

## Autor

**Jaisson De Alba S** — [@byJDSLabs](https://github.com/byJDSLabs)
