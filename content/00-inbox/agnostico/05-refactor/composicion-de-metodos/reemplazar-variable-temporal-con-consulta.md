---
title: Reemplace el Temporizador con la Consulta
tags:
  - poo
  - refactorizacion
  - composicion-de-metodos
course: curso-python/05-refactor
---

# Reemplace el Temporizador con la Consulta

## Problema

coloca el resultado de una expresión en una variable local para su uso posterior en su código.

## Solución

mueva toda la expresión a un método separado y devuelva el resultado de ella. Consulte el método en lugar de usar una variable. Incorpore el nuevo método en otros métodos si es necesario.
