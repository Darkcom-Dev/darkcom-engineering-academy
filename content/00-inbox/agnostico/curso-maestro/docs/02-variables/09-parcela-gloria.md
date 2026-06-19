# La parcela de gloria.

Doña Gloria tiene una parcela de forma perfectamente rectangular. La quiere vender, pero no conoce su área, lo que sí sabe es que uno de los lados del terreno mide X metros y la diagonal mide Y metros

¿Podrías ayudarle haciendo un programa para calcular el área?

#### Entrada:
La entrada contiene dos líneas, la primera con el valor en metros de uno de los lados y la segunda con el valor en metros de la diagonal.
#### Salida:
El mensaje (sin comillas ni tildes) 'El área total es de Z metros cuadrados', donde Z es el valor del área en esa unidad y redondeada a dos cifras decimales.


| Ej. de entrada | Ej. de salida                               |
| -------------- | ------------------------------------------- |
| 20.5 \| 43.8   | El area total es de 793.48 metros cuadrados |

## Teorema de Pitágoras
Sea $a$, $b$ y $c$ las longitudes de los lados de un triángulo rectángulo, con $c$ siendo la longitud de la hipotenusa, entonces:

$$ c^2 = a^2 + b^2 $$

Despejamos el B:

$$ c^2 - a^2 = b^2$$

Ahora despejamos la potencia:

$$ \sqrt{c^2 - a^2} = b$$

```python
def calcular_area_mediante_diagonal_y_un_lado (lado1,diagonal):
    lado = (diagonal**2 - lado1**2)**0.5
    return round(lado * lado1,2)
```

```python
calcular_area_mediante_diagonal_y_un_lado(20.5,43.8)
```


    793.48


