# Component Discovery

## Objetivo

Identificar todos los componentes reutilizables presentes en la interfaz antes de iniciar la migración.

El objetivo es descubrir la arquitectura de componentes implícita en el diseño para garantizar una implementación modular, mantenible y fiel al original.

Esta referencia nunca implementa componentes.

Nunca redefine la arquitectura del proyecto.

Nunca divide componentes arbitrariamente.

Nunca fusiona componentes sin justificación.

Únicamente identifica y documenta la composición de la interfaz.

---

# Principios

## Descubrir, no inventar

Los componentes deben surgir del análisis de la interfaz.

Nunca crear componentes únicamente porque sean comunes en un framework.

La estructura original siempre tiene prioridad.

---

## Pensar en reutilización

Buscar patrones repetitivos.

Si un conjunto de elementos aparece múltiples veces con la misma estructura y comportamiento, probablemente representa un componente reutilizable.

---

## La apariencia no es suficiente

Dos elementos pueden verse iguales y cumplir funciones distintas.

Dos elementos pueden verse diferentes y pertenecer al mismo componente.

Analizar siempre:

- estructura
- comportamiento
- propósito
- contexto

---

## Mantener la composición

La composición de componentes debe mantenerse durante la migración.

Cambiar la tecnología no implica cambiar la organización funcional.

---

# Objetivos del análisis

Responder como mínimo las siguientes preguntas.

## ¿Qué componentes existen?

Identificar todos los componentes visibles.

Ejemplos

- Button
- Card
- Input
- Table
- Sidebar
- Navbar
- Toolbar
- Modal
- Dialog
- Tabs
- Accordion
- Breadcrumb
- Dropdown
- Pagination
- Avatar
- Badge
- Alert
- Toast

No asumir que esta lista es completa.

---

## ¿Qué componentes son reutilizables?

Detectar patrones repetidos.

Buscar coincidencias en

- estructura
- contenido
- comportamiento
- propósito

---

## ¿Qué componentes son únicos?

Identificar componentes específicos que solo aparecen una vez.

No intentar convertirlos automáticamente en componentes reutilizables.

---

## ¿Qué componentes contienen otros componentes?

Analizar composición.

Ejemplo

Dashboard

↓

Sidebar

↓

Navigation

↓

Navigation Item

↓

Icon

↓

Label

---

## ¿Qué variantes existen?

Determinar si un mismo componente posee variantes.

Ejemplo

Button

↓

Primary

Secondary

Danger

Ghost

Outline

Icon

Link

---

## ¿Qué propiedades cambian?

Detectar

- tamaño
- color
- icono
- estado
- contenido
- orientación
- acciones

---

## ¿Qué estados posee cada componente?

Identificar

- Normal
- Hover
- Focus
- Active
- Disabled
- Loading
- Selected
- Error
- Success

---

## ¿Qué dependencias existen?

Determinar si un componente depende de otro para funcionar.

Ejemplo

Table

↓

Pagination

↓

Filter

↓

Search

---

# Elementos que deben analizarse

## Componentes estructurales

Ejemplos

- Layout
- Sidebar
- Header
- Footer
- Main
- Container

---

## Componentes de navegación

Ejemplos

- Navbar
- Menu
- Tabs
- Breadcrumb
- Pagination

---

## Componentes de entrada

Ejemplos

- Input
- Select
- Checkbox
- Radio
- Switch
- Textarea
- DatePicker

---

## Componentes de acción

Ejemplos

- Button
- Floating Action Button
- Icon Button
- Menu Action

---

## Componentes de visualización

Ejemplos

- Card
- Table
- List
- Badge
- Chip
- Avatar
- Timeline
- Progress

---

## Componentes de retroalimentación

Ejemplos

- Alert
- Toast
- Snackbar
- Modal
- Dialog
- Tooltip

---

## Componentes compuestos

Identificar componentes construidos mediante otros componentes.

Ejemplo

User Card

↓

Avatar

↓

Name

↓

Role

↓

Actions

↓

Status Badge

---

# Patrones de composición

Buscar

- composición
- reutilización
- variantes
- especializaciones
- jerarquías

No limitarse a los nombres de los componentes.

---

# Granularidad

Evitar extremos.

No crear un componente para cada elemento HTML.

No crear componentes excesivamente grandes.

La granularidad debe reflejar la intención del diseño.

---

# Errores frecuentes

No dividir componentes únicamente por tamaño.

No fusionar componentes únicamente porque tengan estilos similares.

No asumir reutilización por apariencia.

No crear componentes innecesarios.

No modificar la composición original.

No eliminar componentes intermedios.

No convertir automáticamente layouts completos en un único componente.

No ignorar variantes existentes.

No perder la jerarquía entre componentes.

---

# Resultado esperado

Al finalizar el análisis debe existir un inventario completo de los componentes presentes en la interfaz.

Cada componente debe incluir:

- propósito
- responsabilidad
- composición
- posibles variantes
- estados
- nivel de reutilización
- relaciones con otros componentes

La información obtenida debe permitir implementar una arquitectura de componentes equivalente en cualquier tecnología sin alterar la organización funcional ni la experiencia del usuario.

La arquitectura resultante debe respetar fielmente la composición observada en la interfaz original.