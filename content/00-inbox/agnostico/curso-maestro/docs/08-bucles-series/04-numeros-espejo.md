# Puntos espejo

Dada una lista con N valores numéricos (N > 1), decimos que un punto espejo es aquel índice i (0 < i < N) en el cual la suma de los elementos a la izquierda del índice (sin incluirlo) es igual a la suma de los elementos de la derecha (incluyéndolo).

Así por ejemplo, en la lista [10, 40, 30, 20] hay un punto espejo que corresponde al índice 2 pues 10+40 = 30+20.

Y puede haber más de un punto espejo, por ejemplo en la lista [1, -1, 1, -1, 1, -1] hay 2 puntos espejos: los índices 2 (1+-1 = 1+-1+1+-1) y 4 (1+-1+1+-1 = 1+-1)

### Entrada

La entrada comienza con una línea que contiene el valor de N. Luego siguen N líneas con los elementos de la lista.

### Salida

La salida debe contener una única línea con la cantidad de puntos espejo de la lista.

|Ejemplos de entrada |Ejemplos de salida|
|--------------------|------------------|
|[5, 0.0, 0.0, 0.0, 0.0, 0.0]                   | 4|
|[2, 5.0, 5.0]                                  | 1|
|[8,10.0,-10.0,10.0,-10.0,10.0,-10.0,10.0,-10.0]| 3|

```python

```
