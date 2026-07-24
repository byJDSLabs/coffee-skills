# Migration Validator

## Objetivo

Validar que la implementación final reproduzca fielmente el diseño original antes de considerar la migración como completada.

El propósito de esta validación es detectar cualquier diferencia visual, estructural, funcional o de comportamiento entre el diseño original y la implementación generada.

Esta referencia nunca modifica la implementación.

Nunca corrige errores.

Nunca propone rediseños.

Únicamente identifica diferencias y documenta los hallazgos.

---

# Principios

## El diseño original es la referencia absoluta

Toda validación debe realizarse comparando la implementación con el diseño original.

Nunca validar únicamente contra el código generado.

---

## Validar por evidencia

Cada hallazgo debe estar respaldado por una diferencia observable.

Nunca reportar problemas basados en preferencias personales.

---

## No asumir equivalencia

Dos implementaciones distintas pueden parecer similares.

La validación debe comprobar que producen exactamente la misma experiencia.

---

## Revisar la aplicación completa

No limitar la validación a una única pantalla.

Analizar toda la interfaz disponible.

---

# Alcance de la validación

Verificar como mínimo los siguientes aspectos.

## Estructura

Comparar

- organización
- jerarquía
- composición
- regiones
- distribución

---

## Layout

Comparar

- columnas
- filas
- contenedores
- alineaciones
- tamaños
- posiciones

---

## Componentes

Verificar

- existencia

- composición

- variantes

- estados

- reutilización

---

## Design Tokens

Comparar

- colores

- tipografía

- espaciados

- radios

- sombras

- bordes

- iconografía

---

## Responsive

Validar

- breakpoints

- reorganización

- adaptación

- navegación

- comportamiento

---

## Interacciones

Comprobar

- botones

- formularios

- tablas

- navegación

- overlays

- modales

- validaciones

- retroalimentación visual

---

## Animaciones

Comparar

- transiciones

- duración

- easing

- estados

- microinteracciones

---

## Accesibilidad

Verificar

- navegación por teclado

- foco

- etiquetas

- roles

- contraste

No introducir regresiones.

---

## Rendimiento observable

Comprobar que la implementación no introduce retrasos visibles o comportamientos inesperados respecto al diseño original.

---

# Procedimiento de validación

Realizar la revisión siguiendo el siguiente orden.

1.

Estructura

↓

2.

Layout

↓

3.

Componentes

↓

4.

Design Tokens

↓

5.

Interacciones

↓

6.

Responsive

↓

7.

Accesibilidad

↓

8.

Animaciones

↓

9.

Resultado final

---

# Clasificación de hallazgos

Cada diferencia detectada debe clasificarse como:

## Crítica

Impide considerar la migración como correcta.

Ejemplos

- estructura diferente

- navegación distinta

- componentes faltantes

- responsive incorrecto

---

## Mayor

La funcionalidad existe pero no coincide con el diseño.

Ejemplos

- layout diferente

- espaciados incorrectos

- componentes alterados

---

## Menor

No afecta significativamente la experiencia pero reduce la fidelidad.

Ejemplos

- pequeños desajustes visuales

- diferencias de alineación

- sombras distintas

---

## Observación

Mejora o recomendación sin impacto directo en la fidelidad.

No bloquea la aprobación.

---

# Criterios de aprobación

La migración solo podrá considerarse finalizada cuando:

□ La estructura coincida.

□ El layout coincida.

□ Los componentes coincidan.

□ Los Design Tokens coincidan.

□ Las interacciones coincidan.

□ El comportamiento responsive coincida.

□ No existan diferencias críticas.

□ No existan diferencias mayores sin justificar.

□ Las diferencias menores estén documentadas.

---

# Errores frecuentes

No validar únicamente la apariencia.

No ignorar diferencias pequeñas.

No asumir que un componente equivalente es correcto.

No aceptar cambios de comportamiento.

No aceptar cambios en el flujo de navegación.

No omitir la validación responsive.

No ignorar estados interactivos.

No validar únicamente una resolución.

No validar únicamente una pantalla.

---

# Formato del informe

La validación debe generar un informe estructurado con la siguiente información.

## Resumen

Evaluación general de la migración.

---

## Hallazgos críticos

Lista de diferencias que impiden aprobar la migración.

---

## Hallazgos mayores

Lista de diferencias importantes que deben corregirse.

---

## Hallazgos menores

Lista de diferencias que afectan la fidelidad pero no bloquean completamente la aprobación.

---

## Observaciones

Recomendaciones y comentarios adicionales.

---

## Resultado

Seleccionar uno de los siguientes estados.

✅ Aprobado

La implementación es visual y funcionalmente equivalente al diseño original.

---

⚠️ Aprobado con observaciones

Existen diferencias menores documentadas que no afectan significativamente la experiencia.

---

❌ Requiere correcciones

Se detectaron diferencias críticas o mayores que impiden considerar la migración como finalizada.

---

# Resultado esperado

Al finalizar la validación debe existir evidencia suficiente para determinar objetivamente si la migración reproduce el diseño original.

La decisión de aprobación debe basarse exclusivamente en la comparación entre el diseño fuente y la implementación generada.

La migración solo se considerará exitosa cuando la experiencia visual, estructural y funcional sea prácticamente indistinguible del diseño original.