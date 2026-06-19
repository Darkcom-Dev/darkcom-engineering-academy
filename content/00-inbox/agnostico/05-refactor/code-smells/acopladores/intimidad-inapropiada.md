---
title: Intimidad inapropiada
tags:
  - poo
  - refactorizacion
  - acopladores
course: curso-python/05-refactor
---
# Intimidad inapropiada
## Señales y síntomas

Una clase utiliza los campos y métodos internos de otra clase.

## Razones del problema

Mantenga un ojo cercano en las clases que pasan demasiado tiempo juntas. Las buenas clases deben saber lo menos posible la una de la otra. Tales clases son más fáciles de mantener y reutilizar.

## Tratamiento

La solución más simple es usar Mover método y Mover campo para mover partes de una clase a la clase en la que se utilizan esas partes. Pero esto funciona solo si la primera clase realmente no necesita estas partes.

- Otra solución es usar Extraer clase y Ocultar delegado en la clase para hacer que las relaciones de código sean "oficiales".

- Si las clases son mutuamente interdependientes, debe usar Cambiar asociación bidireccional a unidireccional.

- Si esta "intimidad" está entre una subclase y la superclase, considere Reemplazar delegación con herencia.

## Beneficios

- Mejora la organización del código.
- Simplifica el soporte y la reutilización de código.
