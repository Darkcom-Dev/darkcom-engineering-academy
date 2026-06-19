---
title: Hombre del medio
tags:
  - poo
  - refactorizacion
  - acopladores
course: curso-python/05-refactor
---
# Hombre en el medio
## Señales y síntomas

Si una clase realiza solo una acción, delegando el trabajo a otra clase, ¿por qué existe en absoluto?

## Razones del problema

Este olor puede ser el resultado de una eliminación excesiva de Cadenas de mensajes.

En otros casos, puede ser el resultado del trabajo útil de una clase que se ha ido moviendo gradualmente a otras clases. La clase permanece como una carcasa vacía que no hace nada más que delegar.

## Tratamiento

Si la mayoría de las clases de un método delegan en otra clase, es necesario Eliminar al hombre del medio.

## Beneficios

- Menos código voluminoso.

## Cuándo ignorar

- No elimine a los hombres del medio que se hayan creado por una razón:
- Un hombre del medio puede haberse agregado para evitar dependencias entre clases.
- Algunos patrones de diseño crean hombres del medio a propósito (como Proxy o Decorator).
