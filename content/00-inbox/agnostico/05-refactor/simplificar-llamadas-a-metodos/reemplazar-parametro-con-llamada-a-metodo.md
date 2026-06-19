---
title: Reemplazar Parámetro con Llamada a Método
tags:
  - poo
  - refactorizacion
  - simplificacion-metodos
course: curso-python/05-refactor
---

# Reemplazar Parámetro con Llamada a Método

## Problema
Llamando a un método de consulta y pasando sus resultados como parámetros de otro método, mientras ese método podría llamar a la consulta directamente.

## Solución
En lugar de pasar el valor a través de un parámetro, intente colocar una llamada de consulta dentro del cuerpo del método.
