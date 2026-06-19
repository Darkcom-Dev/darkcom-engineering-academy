---
title: Cambio divergente
tags:
  - poo
  - refactorizacion
  - evitadores-cambio
course: curso-python/05-refactor
---
# Cambio divergente

El cambio divergente se asemeja a la cirugía de escopeta, pero en realidad es el olor opuesto. El cambio divergente se produce cuando se realizan muchos cambios en una sola clase. La cirugía de escopeta se refiere a cuando se realiza un cambio en varias clases al mismo tiempo.

## Signos y síntomas

Te encuentras teniendo que cambiar muchos métodos no relacionados cuando haces cambios en una clase. Por ejemplo, al agregar un nuevo tipo de producto, tienes que cambiar los métodos para encontrar, mostrar y ordenar productos.

## Razones del problema

A menudo, estas modificaciones divergentes se deben a una estructura de programa deficiente o a una programación de "copypasta".

## Tratamiento

Divide el comportamiento de la clase a través de Extract Class.

Si diferentes clases tienen el mismo comportamiento, es posible que desees combinar las clases a través de la herencia (Extract Superclass y Extract Subclass).

## Pago

- Mejora la organización del código.
- Reduce la duplicación de código.
- Simplifica el soporte.
