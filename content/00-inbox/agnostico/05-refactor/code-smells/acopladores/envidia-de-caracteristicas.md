---
title: Envidia de características
tags:
  - poo
  - refactorizacion
  - acopladores
course: curso-python/05-refactor
---
# Envidia de características
## Señales y síntomas

Un método accede más a los datos de otro objeto que a sus propios datos.

## Razones del problema

Este problema puede ocurrir después de que los campos se muevan a una clase de datos. Si este es el caso, es posible que desee mover las operaciones en los datos también a esta clase.

## Tratamiento

Como regla básica, si las cosas cambian al mismo tiempo, debe mantenerlas en el mismo lugar. Por lo general, los datos y las funciones que usan estos datos se cambian juntos (aunque son posibles excepciones).

- Si un método claramente debería moverse a otro lugar, use Mover método.

- Si solo una parte de un método accede a los datos de otro objeto, use Extraer método para mover la parte en cuestión.

- Si un método utiliza funciones de varias otras clases, primero determine qué clase contiene la mayor parte de los datos utilizados. Luego, coloque el método en esta clase junto con los otros datos. Alternativamente, use Extraer método para dividir el método en varias partes que se pueden colocar en diferentes lugares en diferentes clases.

## Beneficios

Menos duplicación de código (si el código de manejo de datos se coloca en un lugar central).

Mejor organización de código (los métodos para manejar datos están junto a los datos reales).

## Cuándo ignorar

A veces, el comportamiento se mantiene deliberadamente separado de la clase que tiene los datos. La ventaja habitual de esto es la capacidad de cambiar dinámicamente el comportamiento (ver patrones de Estrategia, Visitante y otros).
