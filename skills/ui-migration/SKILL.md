---
name: ui-migration
description: Migración pixel-perfect de interfaces de usuario entre tecnologías preservando la máxima fidelidad visual y funcional.
version: 1.0.0
tags: [ui, frontend, migration, pixel-perfect, design-system]
---

# UI Migration

## Cuándo aplicar esta Skill

Aplica las reglas de este documento de forma obligatoria cuando el usuario solicite:
- Migrar componentes, pantallas o interfaces de usuario desde cualquier tecnología u origen hacia otra tecnología objetivo.
- Reorganizar o traducir código de UI preservando la fidelidad visual ("pixel-perfect").
- Reescribir layouts o vistas existentes en un nuevo framework, librería o sistema de estilos sin alterar su apariencia.

---

## Propósito

Esta especialización permite migrar interfaces de usuario desde cualquier tecnología, herramienta o código fuente hacia otra tecnología objetivo preservando la máxima fidelidad visual, estructural y funcional posible.

La prioridad absoluta es reproducir la interfaz original de manera prácticamente idéntica, evitando reinterpretaciones, rediseños o modificaciones creativas.

El diseño original constituye siempre la fuente de verdad.

---

# Objetivos

Esta especialización tiene como objetivos:

- Preservar el diseño original.
- Mantener la estructura visual.
- Mantener la jerarquía de la interfaz.
- Mantener la experiencia del usuario.
- Mantener el comportamiento visual.
- Adaptar únicamente la implementación tecnológica.
- Generar código mantenible y coherente con el stack destino.

---

# Principios Fundamentales

## Fidelidad antes que creatividad

Nunca mejorar el diseño.

Nunca reinterpretar la interfaz.

Nunca reorganizar componentes.

Nunca simplificar el layout.

Nunca modificar colores.

Nunca cambiar tipografías.

Nunca alterar proporciones.

El objetivo no es crear una interfaz mejor.

El objetivo es reconstruir exactamente la interfaz existente utilizando otra tecnología.

---

## Separación entre diseño e implementación

La apariencia visual es independiente de la tecnología utilizada.

El cambio de tecnología nunca debe modificar:

- Layout
- Espaciados
- Tipografía
- Colores
- Sombras
- Bordes
- Iconografía
- Responsive
- Animaciones
- Jerarquía visual

Solo cambia la implementación.

---

## Analizar antes de construir

Nunca generar código inmediatamente.

Siempre realizar primero un análisis completo del proyecto.

Comprender antes de implementar.

---

## Mantener la intención original

La implementación final debe transmitir exactamente la misma intención visual y funcional que la interfaz original.

---

# Flujo General

Antes de implementar cualquier migración, seguir el siguiente flujo:

## Paso 1: Comprender el origen
Identificar:
- Tecnología origen
- Framework
- Herramientas utilizadas
- Arquitectura existente
- Recursos disponibles

---

## Paso 2: Comprender el destino
Identificar:
- Framework destino
- Convenciones del proyecto
- Arquitectura existente
- Restricciones técnicas

---

## Paso 3: Consultar únicamente las referencias necesarias
Las referencias disponibles (si existen en el proyecto) son:
- `knowledge/visual.md` (Composición visual general)
- `knowledge/dom.md` (Estructura lógica del documento)
- `knowledge/design-tokens.md` (Colores, fuentes, espaciados, bordes)
- `knowledge/components.md` (Componentes reutilizables)
- `knowledge/interactions.md` (Comportamiento dinámico)
- `knowledge/responsive.md` (Comportamiento responsive)
- `knowledge/technology-mapper.md` (Mapeo entre stacks)
- `knowledge/pixel-perfect.md` (Garantía de fidelidad visual)
- `knowledge/validator.md` (Verificación final)

No todas las referencias serán necesarias en todas las migraciones. Seleccionar únicamente aquellas relevantes para el contexto.

---

## Paso 4: Construir un plan de migración
El plan debe identificar:
- Componentes
- Dependencias
- Riesgos
- Orden de implementación
- Validaciones

---

## Paso 5: Implementar
Implementar respetando completamente la interfaz original.

---

## Paso 6: Validar
Validar la fidelidad visual. La implementación solo podrá considerarse finalizada cuando la fidelidad visual haya sido verificada.

---

# Formato de Salida Obligatorio

Al ejecutar una migración, la IA debe estructurar su respuesta en el siguiente orden antes de entregar el código final:

1. **Análisis de Origen y Destino:** Resumen breve del stack de entrada y stack de salida.
2. **Tokens de Diseño Detectados:** Identificación de colores, fuentes, espaciados y dimensiones del origen.
3. **Plan de Componentes:** Mapeo de elementos y orden de implementación.
4. **Código Migrado:** Implementación limpia en la tecnología destino.
5. **Checklist de Fidelidad Pixel-Perfect:** Confirmación punto por punto de que no hubo rediseños ni modificaciones creativas.

---

# Reglas Obligatorias

Nunca rediseñar la interfaz.

Nunca cambiar el flujo visual.

Nunca modificar la experiencia del usuario.

Nunca eliminar componentes sin autorización.

Nunca introducir nuevos componentes.

Nunca modificar colores.

Nunca modificar tipografías.

Nunca modificar espaciados.

Nunca modificar tamaños.

Nunca modificar animaciones.

Nunca modificar iconografía.

Nunca asumir comportamientos inexistentes.

Nunca mejorar visualmente la interfaz.

Nunca sustituir componentes únicamente por preferencias personales.

---

# Buenas Prácticas

Analizar completamente antes de implementar.

Trabajar de lo general hacia lo específico.

Detectar patrones reutilizables.

Mantener la consistencia.

Respetar las convenciones del proyecto destino.

Documentar cualquier limitación técnica.

Separar claramente apariencia, comportamiento y estructura.

---

# Restricciones

Esta especialización:

NO redefine requisitos.

NO modifica reglas de negocio.

NO mejora el diseño.

NO realiza optimizaciones visuales.

NO altera la experiencia del usuario.

NO cambia la arquitectura funcional de la aplicación.

NO sustituye componentes por otros equivalentes sin autorización.

---

# Resultado Esperado

Al finalizar la migración:

La interfaz implementada debe ser visualmente indistinguible del diseño original.

La única diferencia aceptable entre ambas implementaciones será la tecnología utilizada.

Todo cambio visual deberá estar explícitamente justificado por una limitación técnica conocida.

La fidelidad visual constituye el principal criterio de éxito de esta especialización.