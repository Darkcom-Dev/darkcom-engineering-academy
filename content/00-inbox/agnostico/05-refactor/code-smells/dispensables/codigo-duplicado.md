---
title: Código Duplicado
tags:
  - poo
  - refactorizacion
  - dispensables
course: curso-python/05-refactor
---
# Código duplicado
## Síntomas

Dos fragmentos de código se ven casi idénticos.

## Razones del problema

La duplicación generalmente ocurre cuando varios programadores están trabajando en diferentes partes del mismo programa al mismo tiempo. Como están trabajando en tareas diferentes, pueden desconocer que su colega ya ha escrito código similar que podría reutilizarse para sus propias necesidades.

También hay duplicación más sutil, cuando partes específicas de código se ven diferentes pero en realidad realizan el mismo trabajo. Este tipo de duplicación puede ser difícil de encontrar y corregir.

A veces la duplicación es intencional. Cuando se corre para cumplir con los plazos y el código existente está "casi correcto" para el trabajo, los programadores novatos pueden no resistir la tentación de copiar y pegar el código relevante. Y en algunos casos, el programador es simplemente demasiado perezoso para despejar el código.

## Tratamiento

Si se encuentra el mismo código en dos o más métodos en la misma clase: use **Extract Method** y coloque las llamadas para el nuevo método en ambos lugares.

- Si se encuentra el mismo código en dos subclases del mismo nivel: Use **Extract Method** para ambas clases, seguido de **Pull Up Field** para los campos utilizados en el método que está extrayendo.

- Si el código duplicado está dentro de un constructor, use **Pull Up Constructor Body**.

- Si el código duplicado es similar pero no completamente idéntico, use **Form Template Method**.

- Si dos métodos hacen lo mismo pero usan algoritmos diferentes, seleccione el mejor algoritmo y aplique **Substitute Algorithm**.

- Si se encuentra código duplicado en dos clases diferentes:

- Si las clases no son parte de una jerarquía, use **Extract Superclass** para crear una única superclase para estas clases que mantenga toda la funcionalidad anterior.

- Si es difícil o imposible una superclase, use **Extract Class** en una clase y use el nuevo componente en la otra.

- Si hay un gran número de expresiones condicionales presentes y realizan el mismo código (diferenciándose solo en sus condiciones), fusiona estos operadores en una sola condición usando **Consolidate Conditional Expression** y use **Extract Method** para colocar la condición en un método separado con un nombre fácil de entender.

- Si se realiza el mismo código en todas las ramas de una expresión condicional: coloque el código idéntico fuera del árbol de condiciones utilizando **Consolidate Duplicate Conditional Fragments**.

## Beneficios

La fusión del código duplicado simplifica la estructura de su código y lo hace más corto.

Simplificación + brevedad = código más fácil de simplificar y más barato de mantener.

## Cuándo ignorar

En casos muy raros, la fusión de dos fragmentos idénticos de código puede hacer que el código sea menos intuitivo y obvio.
