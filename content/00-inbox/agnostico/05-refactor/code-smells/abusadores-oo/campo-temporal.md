---
title: Campo Temporal
tags:
  - poo
  - refactorizacion
  - abusadores-oo
course: curso-python/05-refactor
---
# Campo temporal
## Signos y síntomas

Los campos temporales solo adquieren sus valores (y, por lo tanto, son necesarios para los objetos) en ciertas circunstancias. Fuera de estas circunstancias, están vacíos.

## Razones del problema

A menudo, los campos temporales se crean para su uso en un algoritmo que requiere una gran cantidad de entradas. En lugar de crear una gran cantidad de parámetros en el método, el programador decide crear campos para estos datos en la clase. Estos campos solo se usan en el algoritmo y no se usan el resto del tiempo.

Este tipo de código es difícil de entender. Esperas ver datos en los campos de objetos, pero por alguna razón casi siempre están vacíos.

## Tratamiento

Los campos temporales y todo el código que opera en ellos se pueden poner en una clase separada mediante "Extraer clase". En otras palabras, estás creando un objeto de método, logrando el mismo resultado que si realizaras "Reemplazar método con objeto de método".

Introduce un objeto nulo e intégralo en lugar del código condicional que se usó para verificar la existencia de los valores del campo temporal.

## Pago

Mejora de la claridad y organización del código.
