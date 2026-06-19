# Rubik 2D

El cubo de Rubik es un rompecabezas mecánico tridimensional creado por el escultor y profesor de arquitectura Erno Rubik, de origen húngaro, en 1974. Con cientos de millones de ventas en todo el mundo es considerando, no solo en el rompecabezas, sino en general el juego más vendido de la historia.

Ahora imaginémoslo, pero con las siguientes características:
- En vez de ser tridimensional, solo tendrá dos dimensiones o, visto de otra manera, no tendrá seis caras sino solo una. Por esta razón el objetivo no es que cada cara del cubo quede homogénea, sino cada fila de la única cara
- No necesariamente será 3x3 sino en general de NxN con (2 ≤ N ≤ 9)
- En vez de colores, cada una de las NxN celdas contendrá un número entero entre 1 y N
De esta manera un Rubik en ese formato, con N=4 se verá, una vez ordenado, como:

| 1   | 2   | 3   | 4   |
| --- | --- | --- | --- |
| 1   | 2   | 3   | 4   |
| 1   | 2   | 3   | 4   |
| 1   | 2   | 3   | 4   |

Los desplazamientos permitidos se limitan a mover una fila sea a la derecha o a la
izquierda, y a mover una columna sea hacia arriba o hacia abajo.
Si las filas se numeran consecutivamente de la 1 a la N de arriba a abajo, mientras que
las columnas de izquierda a derecha, mover por ejemplo la fila 2 hacia la derecha daría
como resultado en el Rubik anterior lo siguiente:

| 1   | 2   | 3   | 4   |
| --- | --- | --- | --- |
| 4   | 1   | 2   | 3   |
| 1   | 2   | 3   | 4   |
| 1   | 2   | 3   | 4   |

Luego, mover la columna 3 hacia abajo, daría como resultado:

| 1   | 2   | 3   | 4   |
| --- | --- | --- | --- |
| 4   | 1   | 3   | 3   |
| 1   | 2   | 2   | 4   |
| 1   | 2   | 3   | 4   |

Debes hacer un programa para, dado un Rubik 2D de tamaño N inicialmente ordenado y una serie de movimientos, mostrar cómo quedaría al final.

Todo movimiento se expresa mediante tres elementos: si se trata de fila (F) o columna (C), sobre cuál (entre 1 y N), y si fue a la derecha o abajo (+) o izquierda o arriba (-).

Siendo así, los dos movimientos del ejemplo se expresarían como F2+ y C3+

### Entrada

La entrada comienza con una línea que contiene la cantidad C de casos de prueba (no más de 50). Cada caso se compone de dos líneas, la primera con el valor de N y la segunda con la serie de movimientos realizados (no más de 500) y separados entre sí por un espacio en blanco.

### Salida

La salida debe “dibujar” cada Rubik final según las indicaciones y dejando un renglón en blanco entre uno y otro. Al final del último Rubik también debe haber un renglón en blanco.

**Ejemplo de entrada**
```
3
2
F1-
3
F3+ C2+ F1+
4
F1- F4+ F4+
```

**Ejemplo de salida**
```
21
12

311
123
322

2341
1234
1234
3412
```

```python

```
