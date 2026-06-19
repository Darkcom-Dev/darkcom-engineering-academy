---
title: Obsesión primitiva
tags:
  - poo
  - refactorizacion
  - bloaters
course: curso-python/05-refactor
---
# Obsesión primitiva
## Señales y síntomas

Uso de tipos primitivos en lugar de objetos pequeños para tareas simples (como moneda, rangos, cadenas especiales para números de teléfono, etc.)

Uso de constantes para información de programación (como USER_ADMIN_ROLE = 1 para referirse a usuarios con derechos de administrador).

Uso de constantes de cadena como nombres de campo para usar en matrices de datos.

## Razones del problema

Al igual que la mayoría de los otros "smells" o malos olores, la obsesión por los tipos primitivos nace en momentos de debilidad. "¡Solo un campo para almacenar algunos datos!" dijo el programador. Crear un campo primitivo es mucho más fácil que hacer una clase completamente nueva, ¿verdad? Y así se hizo. Luego se necesitó otro campo y se agregó de la misma manera. La clase se hizo grande e incómoda.

A menudo se utilizan tipos primitivos para "simular" tipos. Entonces, en lugar de un tipo de datos separado, tiene un conjunto de números o cadenas que forman la lista de valores permitidos para alguna entidad. Luego se les dan nombres fáciles de entender a estos números y cadenas específicos a través de constantes, razón por la cual se extienden amplia y lejos.

Otro ejemplo de un mal uso de los tipos primitivos es la simulación de campos. La clase contiene una gran cantidad de datos diversos y se utilizan constantes de cadena (especificadas en la clase) como índices de matriz para obtener estos datos.

## Tratamiento

Si tiene una gran variedad de campos primitivos, puede ser posible agrupar algunos de ellos lógicamente en su propia clase. Incluso mejor, mueva el comportamiento asociado con estos datos a la clase también. Para esta tarea, intente "Replace Data Value with Object".

Si los valores de los campos primitivos se utilizan en parámetros de método, use "Introduce Parameter Object" o "Preserve Whole Object".

Cuando los datos complicados se codifican en variables, use "Replace Type Code with Class", "Replace Type Code with Subclasses" o "Replace Type Code with State/Strategy".

Si hay matrices entre las variables, use "Replace Array with Object".

## Beneficios

El código se vuelve más flexible gracias al uso de objetos en lugar de tipos primitivos.

Mejora la comprensibilidad y organización del código. Las operaciones sobre datos particulares están en el mismo lugar, en lugar de estar dispersas. No más adivinar el motivo de todas estas extrañas constantes y por qué están en una matriz.

Es más fácil encontrar código duplicado.
