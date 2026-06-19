# Faltan 5 pa' las 12

En una familia, en muchas en realidad, es costumbre infaltable escuchar esa canción cada 31 de diciembre. Así mismo, las emisoras radiales se la pasan ese día entero anunciando la cantidad de minutos que faltan para la medianoche. Sin embargo, a veces les da dificultad hacer el cálculo. ¿Podrías ayudarles haciendo un programa que haga ese cálculo por ellos?

#### Entrada:

La entrada contiene tres líneas. La primera contiene un valor entero entre 1 y 12 y corresponde a las horas, la segunda contiene un valor entero entre 0 y 59 y corresponde a los minutos, y la tercera contiene el texto (sin las comillas) 'am' o 'pm'.

Nótese que el inicio del día corresponde a las 12:0 am (12:00 am) y que el máximo valor de la entrada son las 11:59 pm. Por simplicidad, el mediodía no se reporta como 12:0 m (12:00 m) sino como 12:0 pm.

#### Salida:

Una única línea con el mensaje (sin comillas) 'Faltan X para las 12' donde X es la cantidad de minutos faltantes para la medianoche.


| Ej. de entrada | Ej. de salida           |
| -------------- | ----------------------- |
| 11 \| 55 \| pm | Faltan 5 para las 12    |
| 12 \| 0 \| am  | Faltan 1440 para las 12 |
| 12 \| 0 \| pm  | Faltan 720 para las 12  |


```python
def calcular_minutos_restantes(horas, minutos, segundos):
    return (horas * 60 * 60) + (minutos * 60) + segundos
```

```python
calcular_minutos_restantes(11, 55, 0)
```

    42900