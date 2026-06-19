---
title: Clases Alternativas con Interfaces Diferentes
tags:
  - poo
  - refactorizacion
  - abusadores-oo
course: curso-python/05-refactor
---
# Clases alternativas e interfaces diferentes
## Signos y síntomas

Dos clases realizan funciones idénticas pero tienen nombres de métodos diferentes.

## Razones del problema

El programador que creó una de las clases probablemente no sabía que ya existía una clase equivalente funcionalmente.

## Tratamiento

Trate de poner la interfaz de las clases en términos de un denominador común:

- Renombrar métodos para hacerlos idénticos en todas las clases alternativas.
- Mover métodos, agregar parámetros y parametrizar métodos para hacer que la firma y la implementación de los métodos sean iguales.
- Si solo se duplica parte de la funcionalidad de las clases, intente usar Extract Superclass. En este caso, las clases existentes se convertirán en subclases.

Después de determinar qué método de tratamiento usar e implementarlo, es posible que pueda eliminar una de las clases.

## Beneficios

Elimina el código duplicado innecesario, lo que hace que el código resultante sea menos voluminoso.

El código se vuelve más legible y comprensible (ya no tiene que adivinar la razón de creación de una segunda clase que realiza exactamente las mismas funciones que la primera).

## Cuándo ignorar

A veces, fusionar clases es imposible o tan difícil que no tiene sentido. Un ejemplo es cuando las clases alternativas están en bibliotecas diferentes que tienen su propia versión de la clase.
