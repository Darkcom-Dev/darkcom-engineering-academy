---
title: Separar Consulta de Modificador
tags:
  - poo
  - refactorizacion
  - simplificacion-metodos
course: curso-python/05-refactor
---

# Separar Consulta de Modificador

## Problema
¿Tiene un método que devuelve un valor pero también cambia algo dentro de un objeto?

## Solución
Dividir el método en dos métodos separados. Como era de esperar, uno de ellos debería devolver el valor y el otro modificar el objeto.
