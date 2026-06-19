---
title: Reemplazar condicional anidado con cláusulas de guardia
tags:
  - poo
  - refactorizacion
  - simplificacion-de-expresiones-condicionales
course: curso-python/05-refactor
---

# Reemplazar condicional anidado con cláusulas de guardia

## Problema
Tienes un grupo de condicionales anidados y es difícil determinar el flujo normal de ejecución de código.

## Solución
Aisla todas las comprobaciones especiales y casos extremos en cláusulas separadas y colócalas antes de las comprobaciones principales. Idealmente, deberías tener una lista "plana" de condicionales, uno tras otro.
