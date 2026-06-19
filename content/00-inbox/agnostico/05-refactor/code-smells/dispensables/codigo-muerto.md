---
title: Código Muerto
tags:
  - poo
  - refactorizacion
  - dispensables
course: curso-python/05-refactor
---
# Código muerto
## Signos y Síntomas

Una variable, parámetro, campo, método o clase ya no se utiliza (generalmente porque está obsoleto).

## Razones del Problema

Cuando los requisitos del software han cambiado o se han realizado correcciones, nadie tuvo tiempo de limpiar el código antiguo.

Tal código también puede encontrarse en condicionales complejos, cuando una de las ramas se vuelve inalcanzable (debido a un error u otras circunstancias).

## Tratamiento

- La forma más rápida de encontrar código muerto es utilizar un buen IDE.
- Eliminar el código no utilizado y los archivos innecesarios.
- En el caso de una clase innecesaria, se puede aplicar la técnica de "Inline Class" o "Collapse Hierarchy" si se utiliza una subclase o superclase.
- Para eliminar parámetros innecesarios, se debe utilizar la técnica de "Remove Parameter".

## Beneficios

- Reducción del tamaño del código.
- Soporte más sencillo.
