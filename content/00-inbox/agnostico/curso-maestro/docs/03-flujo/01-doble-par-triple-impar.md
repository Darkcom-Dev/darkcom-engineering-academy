# Doble del par o triple del impar

Dado un número entero $N$ se debe mostrar $2*N$ si $N$ es par, o $3*N$ en caso contrario.
#### Entrada:
La entrada contiene una única línea con el valor de N.
#### Salida:
Una única línea con el valor a mostrar según la regla explicada anteriormente.

| Ej. de entrada | Ej. de salida |
| -------------- | ------------- |
| 9              | 27            |


```python
def doble_par_triple_impar(numero):
    if numero % 2 == 0:
        return numero * 2
    else:
        return numero * 3
```

```python
doble_par_triple_impar(9)
```


    27


