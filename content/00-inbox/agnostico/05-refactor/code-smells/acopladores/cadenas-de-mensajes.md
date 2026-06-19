---
title: Cadenas de mensajes
tags:
  - poo
  - refactorizacion
  - acopladores
course: curso-python/05-refactor
---
# Cadenas de mensajes
## Señales y síntomas

En el código se ven una serie de llamadas que se asemejan a `$a->b()->c()->d()`

## Razones del problema

Una cadena de mensajes ocurre cuando un cliente solicita otro objeto, ese objeto solicita otro más, y así sucesivamente. Estas cadenas significan que el cliente depende de la navegación a lo largo de la estructura de clases. Cualquier cambio en estas relaciones requiere modificar el cliente.

## Tratamiento

Para eliminar una cadena de mensajes, use Ocultar delegado.

A veces es mejor pensar por qué se está utilizando el objeto final. Tal vez tendría sentido usar Extraer método para esta funcionalidad y moverlo al principio de la cadena, utilizando Mover método.

## Beneficios

- Reduce las dependencias entre las clases de una cadena.
- Reduce la cantidad de código inflado.

## Cuándo ignorar

Ocultar delegados excesivamente agresivos puede provocar un código en el que es difícil ver dónde se está produciendo realmente la funcionalidad. Lo que es otra forma de decir, evitar también el olor a Hombre del medio.
