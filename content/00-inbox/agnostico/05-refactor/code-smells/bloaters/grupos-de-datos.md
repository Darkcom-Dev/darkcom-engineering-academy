---
title: Grupos de datos
tags:
  - poo
  - refactorizacion
  - bloaters
course: curso-python/05-refactor
---
# Grupos de datos
## Síntomas

A veces, diferentes partes del código contienen grupos idénticos de variables (como parámetros para conectarse a una base de datos). Estos grupos deberían convertirse en sus propias clases.

## Razones del problema

A menudo, estos grupos de datos son el resultado de una estructura de programa deficiente o de una programación "copypasta".

Si quieres asegurarte si algunos datos son un "data clump", simplemente elimina uno de los valores de datos y mira si los demás valores aún tienen sentido. Si este no es el caso, es una buena señal de que este grupo de variables debería combinarse en un objeto.

## Tratamiento

Si los datos repetitivos comprenden los campos de una clase, utiliza "Extract Class" para mover los campos a su propia clase.

Si se pasan los mismos grupos de datos en los parámetros de los métodos, utiliza "Introduce Parameter Object" para separarlos como una clase.

Si algunos de los datos se pasan a otros métodos, piensa en pasar el objeto de datos completo al método en lugar de solo campos individuales. "Preserve Whole Object" te ayudará con esto.

Mira el código utilizado por estos campos. Puede ser una buena idea mover este código a una clase de datos.

## Ventajas

Mejora la comprensión y organización del código. Las operaciones en datos particulares ahora se agrupan en un solo lugar, en lugar de hacerlo de manera aleatoria en todo el código.

Reduce el tamaño del código.

## Cuándo ignorar

Pasar un objeto completo en los parámetros de un método, en lugar de pasar solo sus valores (tipos primitivos), puede crear una dependencia indeseable entre las dos clases.
