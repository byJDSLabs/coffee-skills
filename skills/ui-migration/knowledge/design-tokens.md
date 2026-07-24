# Design Tokens Analysis

## Objetivo

Identificar y documentar todos los Design Tokens presentes en la interfaz para preservar la identidad visual durante el proceso de migración.

Los Design Tokens representan las decisiones de diseño reutilizables de la aplicación, independientemente de la tecnología utilizada para implementarlas.

Esta referencia nunca genera código.

Nunca propone nuevos estilos.

Nunca modifica la identidad visual existente.

Únicamente identifica y documenta los tokens utilizados.

---

# Principios

## La identidad visual debe preservarse

Los Design Tokens representan la esencia visual del producto.

Durante una migración deben mantenerse exactamente iguales, independientemente del framework destino.

---

## Detectar patrones, no valores aislados

No limitarse a listar colores o tamaños.

Buscar relaciones y reutilización.

El objetivo es descubrir el sistema de diseño implícito.

---

## Reutilizar antes que duplicar

Si un mismo color, espaciado o tipografía aparece repetidamente, debe considerarse un token reutilizable y no múltiples valores independientes.

---

## Analizar toda la interfaz

No extraer tokens de un único componente.

Analizar la aplicación completa para detectar consistencia y patrones.

---

# Objetivos del análisis

Responder como mínimo las siguientes preguntas.

## ¿Qué paleta de colores utiliza la aplicación?

Identificar

- Colores primarios
- Colores secundarios
- Colores de acento
- Colores neutros
- Colores de fondo
- Colores de superficie
- Colores de texto
- Colores de borde
- Colores de estados (éxito, error, advertencia, información)

Detectar variaciones y niveles de intensidad.

---

## ¿Qué tipografías utiliza?

Identificar

- Familia tipográfica
- Tamaños
- Pesos
- Altura de línea
- Espaciado entre letras
- Transformaciones (uppercase, lowercase, etc.)

Detectar escalas tipográficas reutilizadas.

---

## ¿Qué sistema de espaciado utiliza?

Identificar

- Márgenes
- Padding
- Separación entre componentes
- Separación entre secciones
- Espaciado interno

Buscar escalas repetidas.

Ejemplo

4

8

12

16

24

32

48

No asumir que todos los valores son independientes.

---

## ¿Qué radios de borde utiliza?

Identificar

- Sin borde redondeado
- Bordes pequeños
- Bordes medianos
- Bordes grandes
- Bordes completamente redondeados

Detectar reutilización.

---

## ¿Qué sombras utiliza?

Identificar

- Elevación baja
- Elevación media
- Elevación alta

Buscar patrones repetidos.

---

## ¿Qué opacidades utiliza?

Identificar

- Hover
- Disabled
- Overlay
- Estados intermedios

---

## ¿Qué tamaños reutiliza?

Detectar

- Alturas de botones
- Alturas de inputs
- Anchuras comunes
- Tamaños de iconos
- Tamaños de avatares

---

## ¿Qué iconografía utiliza?

Identificar

- Biblioteca utilizada
- Tamaños frecuentes
- Colores
- Espaciados

---

## ¿Qué animaciones representan estados?

Detectar

- Duración
- Curvas de aceleración
- Retrasos
- Transiciones

---

# Elementos que deben analizarse

## Colores

Documentar

- Uso
- Frecuencia
- Contexto

No limitarse al valor hexadecimal.

---

## Tipografía

Documentar

- Escalas
- Jerarquías
- Consistencia

---

## Espaciados

Buscar patrones reutilizables.

Evitar documentar valores aislados cuando formen parte de una escala.

---

## Bordes

Analizar

- Grosor
- Color
- Radio

---

## Sombras

Clasificar por nivel de elevación.

---

## Tamaños

Documentar únicamente tamaños reutilizados.

---

## Variables existentes

Si el proyecto ya utiliza variables CSS, SCSS, LESS o Design Tokens, documentarlas y reutilizarlas.

Nunca duplicarlas.

---

# Patrones de diseño

Detectar si existe un sistema consistente.

Por ejemplo

- Escala de 8 puntos
- Escala tipográfica modular
- Paleta basada en Material Design
- Tokens personalizados
- Sistema de colores semánticos

---

# Errores frecuentes

No crear nuevos colores.

No modificar tonos.

No ajustar tamaños "porque se ven mejor".

No cambiar tipografías.

No sustituir iconografía.

No convertir automáticamente valores en variables diferentes.

No duplicar tokens existentes.

No mezclar escalas distintas.

No ignorar patrones reutilizados.

---

# Resultado esperado

Al finalizar el análisis debe existir un inventario completo de los Design Tokens utilizados por la interfaz.

La información obtenida debe permitir reconstruir la identidad visual del proyecto en cualquier tecnología sin alterar su apariencia.

Todos los tokens documentados deben representar decisiones de diseño reutilizables y no valores aislados.

La implementación final debe utilizar estos tokens como fuente única de verdad para garantizar consistencia visual durante toda la migración.