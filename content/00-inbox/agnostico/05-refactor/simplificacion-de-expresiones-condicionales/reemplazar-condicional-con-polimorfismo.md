---
title: Reemplazar condicional con polimorfismo
tags:
  - poo
  - refactorizacion
  - simplificacion-de-expresiones-condicionales
course: curso-python/05-refactor
---

# Reemplazar condicional con polimorfismo

## Problema
Tienes una condición que realiza varias acciones según el tipo o las propiedades del objeto.

## Solución
Crea subclases que coincidan con las ramas del condicional. En ellas, crea un método compartido y mueve el código de la rama correspondiente del condicional a él. Luego, reemplaza el condicional con la llamada al método relevante. El resultado es que se obtendrá la implementación adecuada mediante el polimorfismo según la clase del objeto.
