---
title: Clase perezosa
tags:
  - poo
  - refactorizacion
  - dispensables
course: curso-python/05-refactor
---
# Clase perezosa
## Señales y síntomas

Entender y mantener clases siempre cuesta tiempo y dinero. Entonces, si una clase no hace lo suficiente para ganar su atención, debe ser eliminada.

## Razones del problema

Quizás se diseñó una clase para que fuera completamente funcional, pero después de algunas refactorizaciones se ha vuelto ridículamente pequeña.

O quizás se diseñó para apoyar el trabajo futuro de desarrollo que nunca se realizó.

## Tratamiento

Los componentes que son casi inútiles deben recibir el tratamiento de Inline Class.

Para las subclases con pocas funciones, intente Collapse Hierarchy.

## Pago

- Tamaño de código reducido.
- Mantenimiento más fácil.

## Cuándo ignorar

A veces se crea una Clase Perezosa para delinear intenciones para el desarrollo futuro. En este caso, trate de mantener un equilibrio entre la claridad y la simplicidad en su código.
