---
title: Generación Especulativa
tags:
  - poo
  - refactorizacion
  - dispensables
course: curso-python/05-refactor
---
# Generación especulativa
## Señales y Síntomas

Hay una clase, método, campo o parámetro no utilizado.

## Razones del problema

A veces se crea código "por si acaso" para admitir futuras características anticipadas que nunca se implementan. Como resultado, el código se vuelve difícil de entender y mantener.

## Tratamiento

- Para eliminar clases abstractas no utilizadas, intente Colapsar Jerarquía.
- La delegación innecesaria de funcionalidad a otra clase se puede eliminar a través de la Clase en línea.
- ¿Métodos no utilizados? Utilice Método en línea para deshacerse de ellos.
- Los métodos con parámetros no utilizados deben ser examinados con la ayuda de Eliminar parámetro.
- Los campos no utilizados se pueden eliminar simplemente.

## Beneficios

- Código más ligero.
- Soporte más fácil.

## Cuándo ignorar

Si está trabajando en un marco, es razonable crear funcionalidad que no se usa en el marco en sí, siempre y cuando la funcionalidad sea necesaria para los usuarios del marco.

Antes de eliminar elementos, asegúrese de que no se utilicen en las pruebas unitarias. Esto sucede si las pruebas necesitan una forma de obtener cierta información interna de una clase o realizar acciones especiales relacionadas con las pruebas.
