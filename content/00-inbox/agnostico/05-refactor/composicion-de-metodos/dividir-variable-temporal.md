---
title: Dividir Variable Temporal
tags:
  - poo
  - refactorizacion
  - composicion-de-metodos
course: curso-python/05-refactor
---

# Dividir Variable Temporal

## Problema
tiene una variable local que se utiliza para almacenar varios valores intermedios dentro de un método (excepto para variables de ciclo).

## Solución
use diferentes variables para diferentes valores. Cada variable debe ser responsable de una sola cosa en particular.
