# Cuadrados mágicos

Un cuadrado mágico es una matriz cuadrada en la que la suma de los valores por columnas, filas y diagonales principales es la misma. A ese valor se le llama la constante mágica del cuadrado.


![image.png](00-inbox/ciencia-de-datos/02-recopilacion-de-datos/02-api/assets/image.png)![image-2.png](00-inbox/ciencia-de-datos/02-recopilacion-de-datos/02-api/assets/image-2.png)

<img src='https://es.wikipedia.org/wiki/Cuadrado_m%C3%A1gico'>
Fuente: 

Si bien no tienen ninguna aplicación técnica conocida, se encuentran en muchas obras artísticas y arquitectónicas desde tiempos antiguos. Así por ejemplo, uno de los cuadrados mágicos más famosos es el Alberto Durero, tallado en su obra Melancolía I en el que la constante mágica es 34 y donde las dos cifras centrales de la última fila componen 1514 que corresponde al año de ejecución de la obra.

| 16  | 3   | 2   | 13  |
| --- | --- | --- | --- |
| 5   | 10  | 11  | 8   |
| 9   | 6   | 7   | 12  |
| 4   | 15  | 14  | 1   |

Otro cuadrado mágico famoso es el que se encuentra en la Fachada de la Pasión del Templo Expiatorio de la Sagrada Familia en Barcelona, diseñada por el escultor Josep María Subirachs, en el que la constante mágica es 33, número atribuido a una alusión a la supuesta adscripción masónica, que nunca ha sido demostrada, de Antonio Gaudí, diseñador del templo, ya que 33 son los grados tradicionales de la masonería.

| 1   | 14  | 14  | 4   |
| --- | --- | --- | --- |
| 11  | 7   | 6   | 9   |
| 8   | 10  | 10  | 5   |
| 13  | 2   | 3   | 15  |

¿Dada una serie de cuadrados de 4 x 4, podrías decir si son o no mágicos?

### Entrada

La entrada comienza con una línea que contiene la cantidad N de casos de prueba, no más de 50. Cada caso comienza con una línea que contiene el mensaje (son las comillas)
“Caso i” con i entre 1 y C, seguido por 4 líneas con valores (números enteros positivos) de cada fila del cuadrado separados entre sí por un espacio en blanco.

### Salida

Por cada caso de prueba la salida debe contener una línea con el mensaje (sin comillas ni acento) ‘Cuadrado magico’ o ‘Cuadrado muggle’ según sea el caso.

**Ejemplo de entrada**
```
4

Caso 1
16 3 2 13
5 10 11 8
9 6 7 12
4 15 14 1

Caso 2
1 14 14 4
11 7 6 9
8 10 10 5
13 2 3 15

Caso 3
5 1 1 5
1 1 1 1
1 1 1 1
5 1 1 5

Caso 4
5 1 1 1
1 1 5 1
1 5 1 1
1 1 1 5
```
**Ejemplo de salida**

- Cuadrado magico
- Cuadrado magico
- Cuadrado muggle
- Cuadrado muggle


```python

```
