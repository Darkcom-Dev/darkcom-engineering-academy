---
title: Mover Método
tags:
  - poo
  - refactorizacion
  - mover-funcionalidades-entre-objetos
course: curso-python/05-refactor
---

# Mover Método

## Problema
un método se utiliza más en otra clase que en su propia clase.

## Solución
cree un nuevo método en la clase que más use el método, luego mueva el código del antiguo método allí. Convierta el código del método original en una referencia al nuevo método en la otra clase o elimínelo por completo.
