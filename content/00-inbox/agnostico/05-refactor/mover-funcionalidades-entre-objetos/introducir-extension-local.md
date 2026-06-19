---
title: Introducir Extensión Local
tags:
  - poo
  - refactorizacion
  - mover-funcionalidades-entre-objetos
course: curso-python/05-refactor
---

# Introducir Extensión Local

## Problema
una clase de utilidad no contiene algunos métodos que necesita. Pero no puede agregar estos métodos a la clase.

## Solución
cree una nueva clase que contenga los métodos y haga que sea hija o envoltorio de la clase de utilidad.
