# Visual Analysis

## Objetivo

Analizar exhaustivamente la composición visual de una interfaz antes de iniciar cualquier proceso de migración.

El propósito de este análisis es comprender cómo está construida visualmente la aplicación para poder reproducirla con la máxima fidelidad posible utilizando otra tecnología.

Esta referencia nunca genera código.

Nunca propone mejoras.

Nunca modifica el diseño.

Únicamente documenta la interfaz existente.

---

# Principios

## El diseño original es la fuente de verdad.

Todo análisis debe partir del supuesto de que la interfaz existente representa el resultado esperado.

No deben introducirse mejoras visuales.

No deben realizarse reinterpretaciones.

No deben simplificarse elementos.

No deben reorganizarse componentes.

---

## Analizar antes de concluir

Nunca asumir cómo funciona una interfaz únicamente observando un elemento aislado.

Siempre analizar el contexto completo.

---

## Pensar como un diseñador

Antes de analizar componentes individuales comprender:

- composición
- jerarquía
- ritmo visual
- alineaciones
- agrupaciones
- equilibrio

---

# Objetivos del análisis

Durante el análisis responder como mínimo las siguientes preguntas.

## ¿Cuál es la estructura general?

Por ejemplo

- Dashboard
- Landing Page
- Formulario
- Panel administrativo
- Marketplace
- Sistema empresarial
- Aplicación móvil
- Portal
- Wizard
- CRUD

---

## ¿Cuál es la jerarquía visual?

Identificar

Elemento dominante

Elementos secundarios

Acciones principales

Acciones secundarias

Información prioritaria

Información complementaria

---

## ¿Cómo está distribuido el espacio?

Identificar

- columnas
- filas
- contenedores
- paneles
- márgenes
- padding
- separación entre componentes
- densidad visual

---

## ¿Qué patrones se repiten?

Detectar

- Cards
- Botones
- Inputs
- Listas
- Tablas
- Modales
- Menús
- Paneles
- Cabeceras
- Sidebars
- Toolbars

Identificar cuáles parecen pertenecer a un mismo sistema de diseño.

---

## ¿Qué zonas funcionales existen?

Por ejemplo

Header

Sidebar

Navigation

Toolbar

Filters

Search

Content

Widgets

Footer

Dialogs

Panels

Floating Actions

---

## ¿Cuál es el flujo visual?

Analizar cómo recorre naturalmente la vista del usuario.

Detectar

Punto inicial

Punto de atención principal

Elementos de navegación

Llamados a la acción

Secuencia natural de lectura

---

## ¿Qué nivel de complejidad presenta?

Clasificar

Muy simple

Simple

Media

Alta

Muy alta

Explicar por qué.

---

# Elementos que deben analizarse

## Layout

Determinar

- ancho máximo
- columnas
- grid
- flex
- distribución
- alineación
- responsive aparente

---

## Espaciado

Analizar

- padding
- margin
- separación vertical
- separación horizontal
- ritmo visual

Detectar patrones repetitivos.

---

## Alineaciones

Identificar

- izquierda
- derecha
- centro
- distribución uniforme
- alineaciones compartidas

---

## Escala visual

Detectar

Jerarquía mediante

- tamaño
- peso
- color
- espaciado
- contraste

---

## Balance

Determinar

Si la interfaz es

- simétrica
- asimétrica
- centrada
- distribuida
- modular

---

## Densidad

Clasificar

- Compacta
- Media
- Espaciada

---

## Consistencia

Verificar

Si los componentes mantienen

- tamaños
- márgenes
- alineaciones
- estilos
- patrones

---

# Errores frecuentes

No asumir que dos componentes iguales cumplen la misma función.

No asumir que todos los colores representan acciones.

No modificar proporciones.

No simplificar layouts complejos.

No eliminar espacios "porque parecen demasiado grandes".

No reemplazar componentes por otros más modernos.

No convertir automáticamente layouts a otro patrón.

No centrar elementos que originalmente no estaban centrados.

---

# Resultado esperado

Al finalizar el análisis debe existir una descripción suficientemente completa para que otro agente pueda reconstruir la composición visual sin necesidad de volver a inspeccionar el diseño original.

El resultado debe permitir comprender la interfaz desde una perspectiva puramente visual, independientemente de la tecnología utilizada para implementarla.