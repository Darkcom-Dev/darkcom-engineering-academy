# Matrices triangulares y diagonales

Dada una matriz cuadrada de tamaño N, o sea que tanto la cantidad de filas como de columnas es N, decimos que es triangular superior si todos los elementos por debajo de la diagonal principal son ceros. Entre tanto, decimos que es triangular inferior si ocurre esa situación pero con los elementos por arriba de esa diagonal. En el caso que ocurran las dos cosas al tiempo decimos que es matriz diagonal.

Así por ejemplo, la siguiente matriz 3x3 es triangular superior
```py
1 2 3
0 4 5
0 0 6
```
Mientras que la siguiente matriz 4x4 es triangular inferior
```py
-1 0 0
-1 -1 0
-1 -1 -1
```
Y la siguiente matriz 5x5 es diagonal
```py
8 0 0 0 0
0 8 0 0 0
0 0 8 0 0
0 0 0 8 0
0 0 0 0 8
```
### Entrada
La entrada comienza con una línea que contiene la cantidad C de casos, no más de 50.

Cada caso comienza con una línea que contiene el valor de N (2 ≤ N ≤ 100), seguida por N líneas, cada una con N valores reales separados entre sí por un espacio en blanco.

### Salida

Por cada caso de prueba la salida debe contener una línea con el mensaje (sin comillas) ‘Triangular superior’, ‘Triangular inferior’, ‘Diagonal’, o ‘Ni diagonal ni triangular’ según sea el caso.

**Ejemplo de entrada**
```
4
3
1 1 1
0 1 1
0 0 1
2
8.0 0.0
7.0 6.0
4
1 0 0 0
0 2 0 0
0 0 4 0
0 0 0 8
2
99 98
97 96
```

**Ejemplo de salida**
```
Triangular superior
Triangular inferior
Diagonal
Ni diagonal ni triangular
```

```python

```
