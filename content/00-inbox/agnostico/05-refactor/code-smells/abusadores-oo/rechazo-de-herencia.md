---
title: Rechazo de Herencia
tags:
  - poo
  - refactorizacion
  - abusadores-oo
course: curso-python/05-refactor
---
# Rechazo de herencia
## Signos y Síntomas

Si una subclase solo utiliza algunos de los métodos y propiedades heredados de sus padres, la jerarquía no está bien estructurada. Los métodos no necesarios pueden simplemente quedar sin usar o ser redefinidos y producir excepciones.

## Razones del problema

Alguien quiso crear la herencia entre clases solo para reutilizar el código de una superclase. Pero la superclase y la subclase son completamente diferentes.

## Tratamiento

Si la herencia no tiene sentido y la subclase realmente no tiene nada en común con la superclase, elimine la herencia a favor de Reemplazar Herencia por Delegación.

Si la herencia es apropiada, elimine los campos y métodos no necesarios en la subclase. Extraiga todos los campos y métodos necesarios para la subclase de la clase principal, muévalos a una nueva superclase y configure ambas clases para que hereden de ella (Extraer Superclase).

## Resultado

Mejora la claridad y organización del código. Ya no tendrá que preguntarse por qué la clase Perro hereda de la clase Silla (aunque ambas tengan 4 patas).
