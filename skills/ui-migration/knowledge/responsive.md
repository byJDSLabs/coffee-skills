# Responsive Analysis

## Objetivo

Analizar el comportamiento responsive de la interfaz para comprender cómo se adapta a diferentes tamaños de pantalla durante el proceso de migración.

El objetivo es preservar exactamente la experiencia responsive del diseño original independientemente de la tecnología utilizada para implementarlo.

Esta referencia nunca genera código.

Nunca propone mejoras responsive.

Nunca simplifica el comportamiento existente.

Nunca modifica los puntos de ruptura del diseño.

Únicamente documenta el comportamiento adaptativo observado.

---

# Principios

## El comportamiento responsive forma parte del diseño

La adaptación entre dispositivos es una decisión de diseño.

Debe conservarse durante la migración.

Nunca sustituirla por una implementación diferente únicamente por comodidad.

---

## Analizar múltiples tamaños

Nunca analizar únicamente la versión Desktop.

Siempre estudiar el comportamiento en todos los tamaños disponibles.

---

## Comprender el comportamiento antes de implementarlo

No basta con detectar media queries.

Es necesario entender cómo evoluciona la interfaz entre resoluciones.

---

## Preservar la experiencia del usuario

La navegación, la jerarquía visual y la usabilidad deben mantenerse en todos los dispositivos.

---

# Objetivos del análisis

Responder como mínimo las siguientes preguntas.

## ¿Qué tamaños de pantalla soporta?

Identificar las resoluciones disponibles.

Por ejemplo

- Mobile
- Tablet
- Laptop
- Desktop
- Wide Screen

---

## ¿Qué breakpoints existen?

Detectar los cambios de comportamiento.

No asumir breakpoints estándar.

Identificar únicamente los realmente utilizados.

---

## ¿Cómo cambia el layout?

Analizar

- número de columnas
- distribución
- reorganización
- apilamiento
- expansión
- contracción

---

## ¿Qué componentes cambian?

Identificar

- componentes que desaparecen
- componentes que aparecen
- componentes que cambian de posición
- componentes que cambian de tamaño

---

## ¿Qué elementos permanecen fijos?

Detectar

- Header
- Sidebar
- Navegación
- Botones flotantes
- Barras inferiores

---

## ¿Cómo cambia la navegación?

Analizar

- Menú horizontal
- Menú lateral
- Menú hamburguesa
- Drawer
- Bottom Navigation

---

## ¿Cómo cambian las tablas?

Determinar

- Scroll horizontal
- Colapso
- Cards
- Reorganización
- Ocultamiento de columnas

---

## ¿Cómo cambian los formularios?

Analizar

- número de columnas
- orden
- tamaño de controles
- agrupaciones

---

## ¿Cómo cambia la tipografía?

Detectar

- tamaños
- pesos
- espaciados

Verificar si existen escalas distintas.

---

## ¿Cómo cambian los espacios?

Analizar

- márgenes
- padding
- separación entre componentes

---

## ¿Qué elementos cambian de tamaño?

Identificar

- imágenes
- iconos
- botones
- tarjetas
- paneles

---

## ¿Existen comportamientos especiales?

Detectar

- sticky
- fixed
- overlays
- drawers
- scroll interno
- paneles colapsables

---

# Elementos que deben analizarse

## Layout

Determinar cómo evoluciona el layout entre resoluciones.

---

## Componentes

Documentar cualquier cambio de comportamiento.

---

## Navegación

Analizar la adaptación completa del sistema de navegación.

---

## Contenido

Determinar si cambia

- posición
- tamaño
- orden
- prioridad

---

## Imágenes

Analizar

- escalado
- recorte
- reemplazo
- ocultamiento

---

## Tipografía

Detectar cambios de escala.

---

## Espaciado

Analizar modificaciones en

- padding
- margin
- separación

---

## Scroll

Determinar

- scroll vertical
- scroll horizontal
- scroll interno
- zonas desplazables

---

# Patrones responsive

Identificar estrategias utilizadas.

Por ejemplo

- Mobile First
- Desktop First
- Layout fluido
- Layout fijo
- Grid adaptable
- Flex adaptable

No asumir ninguna estrategia sin evidencia.

---

# Errores frecuentes

No asumir breakpoints estándar.

No eliminar comportamientos responsive.

No convertir layouts complejos en una única columna.

No ocultar contenido sin justificación.

No modificar la prioridad visual.

No reorganizar componentes.

No simplificar formularios.

No alterar la navegación.

No cambiar la experiencia entre dispositivos.

---

# Resultado esperado

Al finalizar el análisis debe existir una descripción completa del comportamiento responsive de la interfaz.

La documentación debe permitir reconstruir exactamente la adaptación de la aplicación en cualquier resolución.

La implementación resultante debe mantener la misma experiencia de usuario, la misma organización visual y el mismo comportamiento observado en el diseño original.

Todos los cambios entre resoluciones deben estar documentados y justificados por el análisis.