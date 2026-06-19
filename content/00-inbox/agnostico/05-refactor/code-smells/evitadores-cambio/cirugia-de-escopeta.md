---
title: Cirugía de Escopeta
tags:
  - poo
  - refactorizacion
  - evitadores-cambio
course: curso-python/05-refactor
---
# Cirugía de escopeta

La cirugía de escopeta se asemeja al Cambio Divergente, pero en realidad es el olor opuesto. El Cambio Divergente se produce cuando se realizan muchos cambios en una sola clase. La cirugía de escopeta se refiere a cuando se realiza un cambio en varias clases al mismo tiempo.

## Signos y síntomas

Realizar cualquier modificación requiere que hagas muchos pequeños cambios en muchas clases diferentes.

## Razones del problema

Una sola responsabilidad se ha dividido entre un gran número de clases. Esto puede suceder después de una aplicación excesiva del Cambio Divergente.

## Tratamiento

Usa Move Method y Move Field para mover los comportamientos de clase existentes a una sola clase. Si no hay una clase adecuada para esto, crea una nueva.

Si mover el código a la misma clase deja las clases originales casi vacías, intenta deshacerte de estas clases redundantes mediante Inline Class.

## Pago

- Mejora la organización.
- Menos duplicación de código.
- Mantenimiento más fácil.
