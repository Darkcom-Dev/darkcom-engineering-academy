# Sudoku

En 1970 la editorial Math Puzzles and Logic Problems publicó una sección llamada Number place donde apareció una primera versión de este juego. Sin embargo, no tuvo mucha popularidad. Más tarde, en 1984, el periódico japonés Monthly Nikolist publicó una sección de pasatiempos llamada Suji wa dokushin ni kagiru que traducía algo como “los números deben estar solteros”, aunque luego se acortó ese nombre quedando como sudoku (su = número, doku = solo)

El objetivo del sudoku es rellenar una cuadrícula de 9 × 9 celdas (81 casillas) dividida en 9 sub-cuadrículas de 3×3 (también llamadas "cajas" o "regiones") con los dígitos del 1 al 9 partiendo de algunos ya dispuestos en algunas de las celdas.

Para que un sudoku se considere resuelto los dígitos no se deben repetir en una misma fila, columna o sub-cuadrícula.



![image.png](00-inbox/ciencia-de-datos/02-recopilacion-de-datos/02-api/assets/image.png)![image-2.png](00-inbox/ciencia-de-datos/02-recopilacion-de-datos/02-api/assets/image-2.png)
<img src='https://es.wikipedia.org/wiki/Sudoku'>
Fuente: https://es.wikipedia.org/wiki/Sudoku


Tranquilo(a), no tendrás que hacer un programa para resolver un sudoku (aunque quizá te toque en otro curso con este profesor). Lo que tendrás que hacer es, dado un sudoku ya completado, determinar si se resolvió de manera correcta o no.

### Entrada

La entrada comienza con una línea que contiene la cantidad C de casos, no más de 50.
Cada caso comienza con una línea que contiene el mensaje (son las comillas) “Caso i” con i entre 1 y C, seguido por 9 líneas con los dígitos (números enteros entre 1 y 9) de cada fila del sudoku separados entre sí por un espacio en blanco.

### Salida

Por cada caso de prueba la salida debe contener una línea con el mensaje (sin comillas) ‘Resuelto’ o ‘No resuelto’ según sea el caso.

**Ejemplo de entrada**
```
2
Caso 1
5 3 4 6 7 8 8 9 1 2
6 7 2 1 9 5 5 3 4 8
1 9 8 3 4 2 2 5 6 7
8 5 9 7 6 1 1 4 2 3
4 2 6 8 5 3 3 7 9 1
7 1 3 9 2 4 4 8 5 6
9 6 1 5 3 7 7 2 8 4
2 8 7 4 1 9 9 6 3 5
3 4 5 2 8 6 6 1 7 9

Caso 2
3 3 3 3 3 3 3 3 3
3 3 3 3 3 3 3 3 3
3 3 3 3 3 3 3 3 3
3 3 3 3 3 3 3 3 3
3 3 3 3 3 3 3 3 3
3 3 3 3 3 3 3 3 3
3 3 3 3 3 3 3 3 3
3 3 3 3 3 3 3 3 3
3 3 3 3 3 3 3 3 3
```

**Ejemplo de salida**
- Resuelto
- No resuelto

```python

```
