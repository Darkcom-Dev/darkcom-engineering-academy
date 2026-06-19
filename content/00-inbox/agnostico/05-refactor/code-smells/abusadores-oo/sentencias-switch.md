---
title: Sentencias Switch
tags:
  - poo
  - refactorizacion
  - abusadores-oo
course: curso-python/05-refactor
---
# Sentencias switch
## Signos y síntomas

Tienes un operador de switch complejo o una secuencia de declaraciones if.

## Razones del problema

El uso relativamente poco común de los operadores de switch y case es una de las características del código orientado a objetos. A menudo, el código para un solo switch puede estar disperso en diferentes lugares del programa. Cuando se agrega una nueva condición, debes encontrar todo el código del switch y modificarlo.

Como regla general, cuando ves switch, deberías pensar en polimorfismo.

## Tratamiento

Para aislar el switch y colocarlo en la clase correcta, es posible que necesites utilizar "Extraer método" y luego "Mover método".

Si el switch se basa en el código de tipo, como cuando se cambia el modo de tiempo de ejecución del programa, usa "Reemplazar código de tipo con subclases" o "Reemplazar código de tipo con estado/estrategia".

Después de especificar la estructura de herencia, usa "Reemplazar condicional con polimorfismo".

Si no hay demasiadas condiciones en el operador y todas llaman al mismo método con diferentes parámetros, el polimorfismo será superfluo. En este caso, puedes dividir ese método en varios métodos más pequeños con "Reemplazar parámetro con métodos explícitos" y cambiar el switch en consecuencia.

Si una de las opciones condicionales es nula, usa "Introducir objeto nulo".

## Pago

Mejora de la organización del código.

## Cuándo ignorar

Cuando un operador de switch realiza acciones simples, no hay razón para realizar cambios en el código.

A menudo, los operadores de switch son utilizados por patrones de diseño de fábrica (Método de fábrica o Fábrica abstracta) para seleccionar una clase creada.
