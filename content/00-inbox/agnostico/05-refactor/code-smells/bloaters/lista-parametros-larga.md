---
title: Lista de parámetros larga
tags:
  - poo
  - refactorizacion
  - bloaters
course: curso-python/05-refactor
---
# Lista de parámetros larga
## Síntomas

Más de tres o cuatro parámetros para un método.

## Razones del problema

Una lista de parámetros larga puede suceder después de que varios tipos de algoritmos se fusionen en un solo método. Se puede crear una lista larga para controlar qué algoritmo se ejecutará y cómo.

Las listas largas de parámetros también pueden ser el resultado de esfuerzos para hacer que las clases sean más independientes entre sí. Por ejemplo, el código para crear objetos específicos necesarios en un método se movió del método al código que llama al método, pero los objetos creados se pasan al método como parámetros. Por lo tanto, la clase original ya no conoce las relaciones entre objetos y la dependencia ha disminuido. Pero si se crean varios de estos objetos, cada uno requerirá su propio parámetro, lo que significa una lista de parámetros más larga.

Es difícil entender tales listas, que se vuelven contradictorias y difíciles de usar a medida que se vuelven más largas. En lugar de una lista larga de parámetros, un método puede usar los datos de su propio objeto. Si el objeto actual no contiene todos los datos necesarios, se puede pasar otro objeto (que obtendrá los datos necesarios) como parámetro del método.

## Tratamiento

Verifique qué valores se pasan a los parámetros. Si algunos de los argumentos son solo resultados de llamadas de método de otro objeto, use Reemplazar parámetro con llamada de método. Este objeto se puede colocar en el campo de su propia clase o pasar como parámetro del método.

En lugar de pasar un grupo de datos recibidos de otro objeto como parámetros, pase el objeto en sí mismo al método, usando Preservar todo el objeto.

Pero si estos parámetros provienen de diferentes fuentes, se pueden pasar como un solo objeto de parámetro a través de Introducir objeto de parámetro.

## Resultado

Código más legible y más corto.

La refactorización puede revelar código duplicado previamente no detectado.

## Cuándo ignorar

No se deshaga de los parámetros si hacerlo causaría una dependencia no deseada entre clases.
