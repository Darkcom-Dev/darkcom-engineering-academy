---
title: Clase de biblioteca incompleta
tags:
  - poo
  - refactorizacion
  - otros
course: curso-python/05-refactor
---
# Clase biblioteca incompleta
## Signos y síntomas

Tarde o temprano, las bibliotecas dejan de satisfacer las necesidades del usuario. 
La única solución al problema, cambiar la biblioteca, a menudo es imposible 
ya que la biblioteca es de solo lectura.

## Razones del problema

El autor de la biblioteca no ha proporcionado las funciones que necesita 
o se ha negado a implementarlas.

## Tratamiento

Para introducir algunos métodos en una clase de biblioteca, 
use "Introducir método externo" (Introduce Foreign Method).

Para cambios importantes en una biblioteca de clases, use "Extensión local" 
(Introduce Local Extension).

## Beneficios

Reduce la duplicación de código (en lugar de crear su propia biblioteca desde cero, 
todavía puede aprovechar una existente).

## Cuándo ignorar

Extender una biblioteca puede generar trabajo adicional si los cambios en la 
biblioteca involucran cambios en el código.
