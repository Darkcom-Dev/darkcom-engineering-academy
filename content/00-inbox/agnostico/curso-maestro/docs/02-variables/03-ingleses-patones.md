# Ingleses patones.
  
¿Conoces el sistema de medida inglés? En el caso de las distancias, en vez de usar el sistema métrico, como todo el mundo, usan medidas como pulgadas, pies o yardas. Y en el caso de los pies parece que los ingleses de la antigüedad necesitaban un calzado
bastante grande puesto que un pie equivale a 30.48 centímetros. Dejando ese dato curioso de lado, ¿puedes hacer un programa para convertir una medida en pies a su equivalente en centímetros?

#### Entrada:
La entrada contiene una única línea con un valor real no negativo (expresado en pies)
#### Salida:
La equivalencia de ese valor en centímetros.


| Ej. de entrada | Ej. de salida |
| -------------- | ------------- |
| 5.75           | 175.26        |


```python
def conversor_pies_a_centimetros(pies):
    return pies * 30.48
```

```python
conversor_pies_a_centimetros(5.75)
```


    175.26


