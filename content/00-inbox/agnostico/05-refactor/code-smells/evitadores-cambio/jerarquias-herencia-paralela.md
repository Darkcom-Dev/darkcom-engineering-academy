---
title: Jerarquías de herencia paralela
tags:
  - poo
  - refactorizacion
  - evitadores-cambio
course: curso-python/05-refactor
---
# Jerarquías y herencia paralela
## Signos y síntomas

Cada vez que creas una subclase para una clase, te encuentras necesitando crear una subclase para otra clase.

## Razones del problema

Todo estaba bien mientras la jerarquía permanecía pequeña. Pero con la adición de nuevas clases, hacer cambios se ha vuelto más y más difícil.

## Tratamiento

Puedes reducir la duplicación de código de jerarquías de clases paralelas en dos pasos. Primero, haz que las instancias de una jerarquía se refieran a las instancias de otra jerarquía. Luego, elimina la jerarquía en la clase referida, usando "Mover método" y "Mover campo".

## Pago

Reduce la duplicación de código.
Puede mejorar la organización del código.

## Cuando ignorarlo

A veces, tener jerarquías de clases paralelas es solo una forma de evitar un lío aún mayor con la arquitectura del programa. Si descubres que tus intentos de reducir las jerarquías producen un código aún más feo, simplemente déjalo así, revierte todos tus cambios y acostúmbrate a ese código.
