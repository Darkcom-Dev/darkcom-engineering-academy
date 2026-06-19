# Determinando el cuadrante.

Un plano cartesiano bi-dimensional puede dividirse en cuatro cuadrantes como se muestra en la figura.

<img src="02-2D-cartesian-coordinates.svg" alt="Plano cartesiano">

El problema consiste en determinar, para una coordenada (X, Y) a cuál cuadrante pertenece, si está sobre alguno de os ejes, o si se trata del origen.

#### Entrada:

La entrada contiene dos líneas, cada una con un valor real, la primera para X y la segunda para Y.

#### Salida:

Una única línea con el mensaje (sin comillas ni puntuación): 'La coordenada (X, Y) se encuentra en el cuadrante J', o 'La coordenada (X, Y) esta sobre el eje K', o 'La coordenada (X, Y) corresponde al origen'.

| Ej. de entrada | Ej. de salida                                               |
| -------------- | ----------------------------------------------------------- |
| -5.84,-1.37    | La coordenada (-5.84, -1.37) se encuentra en el cuadrante 3 |
| 0.0, 0.0       | La coordenada (0.0, 0.0)corresponde al origen               |
| 7.9, 0.0       | La coordenada (7.9, 0.0) está sobre el eje X                |


```python
def determinando_cuadrante(x, y):
    if x > 0 and y > 0:
        print("La coordenada ({}, {}) se encuentra en el cuadrante 1".format(x, y))
    elif x < 0 and y > 0:
        print("La coordenada ({}, {}) se encuentra en el cuadrante 2".format(x, y))
    elif x < 0 and y < 0:
        print("La coordenada ({}, {}) se encuentra en el cuadrante 3".format(x, y))
    elif x > 0 and y < 0:
        print("La coordenada ({}, {}) se encuentra en el cuadrante 4".format(x, y))
    elif x == 0 and y != 0:
        print("La coordenada ({}, {}) se encuentra sobre el eje Y".format(x, y))
    elif x != 0 and y == 0:
        print("La coordenada ({}, {}) se encuentra sobre el eje X".format(x, y))
    else:
        print("La coordenada ({}, {}) corresponde al origen".format(x, y))
```

```python
determinando_cuadrante(7.9, 0)
```

    La coordenada (7.9, 0) se encuentra sobre el eje X

