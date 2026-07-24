# Technology Mapper

## Objetivo

Determinar la estrategia de implementación más adecuada para reproducir la interfaz original utilizando la tecnología destino.

El objetivo es adaptar únicamente la implementación técnica sin modificar la apariencia, el comportamiento ni la experiencia del usuario.

Esta referencia nunca redefine el diseño.

Nunca modifica la arquitectura funcional.

Nunca propone cambios visuales.

Únicamente traduce una implementación hacia otra tecnología.

---

# Principios

## La tecnología es un medio, no un objetivo

El framework utilizado no debe influir en el resultado visual.

Angular, React, Vue, Svelte, Flutter o cualquier otra tecnología deben producir exactamente la misma interfaz.

---

## Mantener la intención original

La implementación debe preservar:

- composición
- estructura
- comportamiento
- responsive
- identidad visual
- accesibilidad

---

## Adaptar sin reinterpretar

No convertir una solución únicamente porque el framework tenga una forma diferente de implementarla.

El comportamiento observado siempre tiene prioridad.

---

## Respetar la arquitectura destino

La implementación debe integrarse con las convenciones y buenas prácticas del proyecto destino.

Nunca imponer patrones ajenos al ecosistema.

---

# Objetivos del análisis

Responder como mínimo las siguientes preguntas.

## ¿Cuál es la tecnología origen?

Identificar

- HTML
- CSS
- JavaScript
- Angular
- React
- Vue
- Svelte
- Flutter
- React Native
- Figma Export
- v0
- Stitch
- OpenDesign
- Builder.io
- Webflow

Comprender cómo fue construida la interfaz.

---

## ¿Cuál es la tecnología destino?

Identificar

- Framework

- Librerías

- Sistema de componentes

- Sistema de estilos

- Herramientas disponibles

---

## ¿Qué arquitectura utiliza el proyecto destino?

Analizar

- estructura de carpetas

- componentes

- routing

- estado

- servicios

- convenciones

Nunca ignorar la arquitectura existente.

---

## ¿Qué sistema de estilos utiliza?

Identificar

- CSS

- SCSS

- Tailwind

- CSS Modules

- Styled Components

- Emotion

- Material

- Bootstrap

- otro

Respetar siempre el sistema existente.

---

## ¿Qué sistema de componentes utiliza?

Determinar

- componentes propios

- librerías

- design system

- componentes reutilizables

Priorizar la reutilización antes de crear nuevos componentes.

---

## ¿Qué patrones deben adaptarse?

Detectar

- navegación

- formularios

- tablas

- modales

- overlays

- layouts

- grids

- responsive

---

## ¿Qué diferencias existen entre ambas tecnologías?

Analizar únicamente diferencias de implementación.

Nunca diferencias visuales.

---

# Estrategia de adaptación

La migración debe realizarse respetando el siguiente orden.

1.

Arquitectura

↓

2.

Componentes

↓

3.

Layout

↓

4.

Estilos

↓

5.

Interacciones

↓

6.

Responsive

↓

7.

Optimización

---

# Adaptaciones permitidas

Es válido adaptar

- sintaxis

- estructura del proyecto

- convenciones

- organización de carpetas

- APIs del framework

Siempre que el resultado visual permanezca idéntico.

---

# Adaptaciones no permitidas

Nunca cambiar

- colores

- tamaños

- márgenes

- padding

- tipografía

- iconografía

- composición

- responsive

- animaciones

- flujo visual

- comportamiento

---

# Compatibilidad

Cuando una funcionalidad no exista en la tecnología destino

Analizar

- alternativas equivalentes

- limitaciones

- impacto

Documentar cualquier diferencia.

Nunca ocultarla.

---

# Reutilización

Antes de crear cualquier componente nuevo

Buscar

- componentes existentes

- utilidades existentes

- estilos existentes

- tokens existentes

- helpers existentes

Priorizar siempre la reutilización.

---

# Rendimiento

La implementación debe respetar las buenas prácticas del framework destino.

Sin embargo

Nunca sacrificar fidelidad visual únicamente por optimización prematura.

---

# Accesibilidad

La migración debe preservar

- navegación por teclado

- etiquetas

- roles

- estados

- contraste

- foco

No introducir regresiones de accesibilidad.

---

# Errores frecuentes

No reemplazar componentes únicamente porque el framework tenga otros equivalentes.

No simplificar layouts.

No eliminar wrappers.

No modificar la estructura.

No cambiar la organización visual.

No crear componentes innecesarios.

No romper la arquitectura existente.

No ignorar convenciones del proyecto.

No adaptar el diseño al framework.

El framework debe adaptarse al diseño.

---

# Resultado esperado

La implementación final debe sentirse como una aplicación desarrollada originalmente en la tecnología destino.

Al mismo tiempo, debe ser visual y funcionalmente indistinguible de la implementación original.

La única diferencia aceptable entre ambas aplicaciones será la tecnología utilizada para construirlas.

Toda decisión de adaptación debe estar respaldada por una necesidad técnica y nunca por preferencias personales.