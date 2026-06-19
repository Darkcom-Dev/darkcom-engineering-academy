# Mediana de tres valores

La mediana (del latín medianus que significa 'del medio') de un conjunto de datos, representa del dato que estaría en la posición central si estos estuvieran ordenados y si la cantidad de datos es impar.

En el caso que el número de datos sea par, lo que cambia es que se calcula como el valor medio de los dos del centro.

Así por ejemplo en el conjunto de datos {8, 10, 4, 6, 2}, la mediana es el 6.

Escribe un programa para que dados tres valores reales (no necesariamente distintos), se muestre la mediana de los tres.

#### Entrada:
La entrada contiene tres lineas, cada una con un valor real.
#### Salida:
El mensaje 'sin comillas''X es la mediana'


| Ej. de entrada   | Ej. de salida      |
| ---------------- | ------------------ |
| 3.0, 5.0, 4.0    | 4.0 es la mediana  |
| 12.5, 9.8, 12.5  | 12.5 es la mediana |
| 13.3, 13.3, 13.3 | 13.3 es la mediana |


```python
def mediana3valores(a, b, c):
    list = [a, b, c]
    list.sort()
    print(list[1])
```

```python
mediana3valores(12.5, 9.8, 12.5)
```

    12.5