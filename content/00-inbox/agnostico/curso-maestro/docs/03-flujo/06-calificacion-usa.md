# Calificación made in USA

Si algo se aprendió de las películas norteamericanas es que, cuando de calificaciones se trata, 'A' es lo máximo y que 'F' significa perder (en algunas ocasiones también se aprendió que falsificar una 'F' por una 'B' es mucho más fácil y creíble que hacerlo por una A). pero ¿Qué significa exactamente esa escala?

Si consideramos la calificación como un porcentaje entre **0% y 100%**, la equivalencia con la escala norteamericana (aunque otros países también la adoptan) es la siguiente:

- **'A'** significa mayor o igual a 90
- **'B'** significa mayor o igual a 80, pero menor a 90
- **'C'** significa mayor o igual a 70, pero menor a 80
- **'D'** significa mayor o igual a 60, pero menor a 70
- **'E'** significa mayor o igual a 40, pero menor a 60
- **'F'** significa menor a 40

Debes escribir un programa para determinar, dada una calificación expresada en porcentaje, su correspondencia en esta escala.

#### Entrada:
La entrada contiene una única línea con un valor real X correspondiente al porcentaje.
#### Salida:
Una única línea con el mensaje (sin comillas ni puntuación): 'El porcentaje X corresponde a la calificacion Y'.


| Ejemplos de entrada | Ejemplos de salida                                  |
| ------------------- | --------------------------------------------------- |
| 50.5                | El porcentaje 50.5 corresponde a la calificacion E  |
| 100.0               | El porcentaje 100.0 corresponde a la calificacion A |
| 0.0                 | El porcentaje 0.0 corresponde a la calificacion F   |

```python
def calificacion_USA(porcentaje):
    if porcentaje >= 90:
        return 'A'
    elif porcentaje >= 80 and porcentaje < 90:
        return 'B'
    elif porcentaje >= 70 and porcentaje < 80:
        return 'C'
    elif porcentaje >= 60 and porcentaje < 70:
        return 'D'
    elif porcentaje >= 40 and porcentaje < 60:
        return 'E'
    else:
        return 'F'
```

```python
porcentaje = 50.5
print(f'el porcentaje {porcentaje} corresponde a la calificacion {calificacion_USA(porcentaje=porcentaje)}')
```

    el porcentaje 50.5 corresponde a la calificacion E


```python
calificacion_dict = {
    'A' : range(90,100),
    'B' : range(80,89),
    'C' : range(70,79),
    'D' : range(60,69),
    'E' : range(40,59),
    'F' : range(0,39),
}

def calificacion_USA2(porcentaje):
    if porcentaje in calificacion_dict['A']:
        return 'A'
    elif porcentaje in calificacion_dict['B']:
        return 'B'
    elif porcentaje in calificacion_dict['C']:
        return 'C'
    elif porcentaje in calificacion_dict['D']:
        return 'D'
    elif porcentaje in calificacion_dict['E']:
        return 'E'
    else:
        return 'F'
    
porcentaje = 50.5
print(f'el porcentaje {porcentaje} corresponde a la calificacion {calificacion_USA2(porcentaje=porcentaje)}')
```

    el porcentaje 50.5 corresponde a la calificacion F


```python
50 in range(51,59)
```

    False


