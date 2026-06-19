---
title: Método largo
tags:
  - poo
  - refactorizacion
  - bloaters
course: curso-python/05-refactor
---
# Método largo
## Síntomas

Un método contiene demasiadas líneas de código. Por lo general, cualquier método que tenga más de diez líneas debería hacer que te preguntes por qué.

## Causas del problema

Algo se agrega al método constantemente, pero nunca se elimina nada. Dado que es más fácil escribir código que leerlo, este "olor" permanece inadvertido hasta que el método se convierte en una enorme y poco manejable bestia.

Mentalmente, a menudo es más difícil crear un nuevo método que agregar a uno existente. "Pero son solo dos líneas, no hay razón para crear todo un método para eso..." lo que significa que se agrega otra línea y luego otra más, dando origen a un enredo de código espagueti.

## Tratamiento

Como regla general, si sientes la necesidad de comentar algo dentro de un método, deberías tomar ese código y ponerlo en un nuevo método. Incluso una sola línea puede y debe ser separada en un método diferente si requiere explicaciones. Y si el método tiene un nombre descriptivo, nadie necesitará mirar el código para ver lo que hace.

Para reducir la longitud de un cuerpo de método, usa Extract Method.

Si las variables locales y los parámetros interfieren con la extracción de un método, usa Replace Temp with Query, Introduce Parameter Object o Preserve Whole Object.

Si ninguna de las recetas anteriores ayuda, intenta mover todo el método a un objeto separado a través de Replace Method with Method Object.

Los operadores condicionales y los bucles son una buena pista de que el código se puede mover a un método separado. Para los condicionales, usa Decompose Conditional. Si los bucles están en el camino, intenta Extract Method.

## Beneficios

Entre todos los tipos de código orientado a objetos, las clases con métodos cortos viven más tiempo. Cuanto más largo es un método o función, más difícil se vuelve entenderlo y mantenerlo.

Además, los métodos largos ofrecen el lugar perfecto para esconder código duplicado no deseado.

### Rendimiento

¿El aumento en el número de métodos perjudica el rendimiento, como afirman muchas personas? En casi todos los casos, el impacto es tan insignificante que ni siquiera vale la pena preocuparse por ello.

Además, ahora que tienes código claro y comprensible, es más probable que encuentres métodos verdaderamente efectivos para reestructurar el código y obtener verdaderas mejoras de rendimiento si surge la necesidad.
