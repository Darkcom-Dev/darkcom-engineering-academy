# Sumando y quizá restando.

Dado un valor X, incrementarlo en 5 unidades y mostrar el resultado. Pero espera, no puede ser tan sencillo. Si dicho valor X es superior a 10, mostrar también el resultado de disminuirlo en 8 unidades.
#### Entrada:
La entrada contiene una única línea con un valor real.
#### Salida:
Una o dos líneas. La primera con el valor de X incrementado en 5 y, opcionalmente la segunda con el valor de X disminuido en 8.


| Ej. de entrada | Ej. de salida |
| -------------- | ------------- |
| -2.8           | 2.2           |
| 15             | 20.0 \| 7.0   |
| 10.0           | 15.0          |


```python
def sumando_restando(numero):
    x = float(numero)
    x += 5
    print(x)
    if numero > 10:
        x = float(numero - 8)
        print(x)
    
```

```python
sumando_restando(15)
```

    20.0
    7.0

