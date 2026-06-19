# Ganancia, pérdida o desperdicio de tiempo

A grandes rasgos, una inversión funciona más o menos así: se invierte un **capital inicial** $C$ y pasado un determinado tiempo se obtiene un **retorno** $K$. La idea siempre es que $K$ sea mayor que $C$, y entre más mejor, pero ese no siempre es el caso.

Así por ejemplo, si $C$ es $80000 y $K$ es $90000 decimos que hubo una ganancia de $10000, que corresponde a decir que hubo una ganancia del 12.5%

En otro ejemplo, si $C$ es $80000 y $K$ es $50000 decimos que hubo una pérdida de $30000, que corresponde a decir que hubo una pérdida del 37.5%

Recordemos que ese porcentaje (la variación del capital respecto al retorno obtenido) se calcula como:

$$ 
\frac{K−C}{C} ∗ 100% = vC
$$

Escribe un programa para dados dos valores enteros positivos para C y K mostrar de cuánto es la ganancia o la pérdida y a qué porcentaje corresponde. Si no hay ni ganancia ni pérdida, también debe decirse.

#### Entrada:

La entrada contiene dos líneas, la primera con el valor de $C$ y la segunda con el de $K$.

#### Salida:

Una única línea con el mensaje a mostrar, el cual será uno de los siguientes tres (sin comillas ni tildes):
   
- 'Hubo una ganancia de $ X correspondiente al Y % del capital invertido'
- 'Hubo una perdida de $ X correspondiente al Y % del capital invertido'
- 'No hubo ni ganancia ni perdida, la inversion fue un desperdicio de tiempo, pero al menos no de dinero'

<table>
    <tr>
        <th>Ejemplos de entrada</th>
        <th>Ejemplos de salida</th>
    </tr>
    <tr>
        <td>80000<br>84000</td>
        <td>Hubo una ganancia de $ 4000 correspondiente al 5.0 % del capital invertido</td>
    </tr>
    <tr>
        <td>80000<br>50000</td>
        <td>Hubo una perdida de $ 30000 correspondiente al 37.5 % del capital invertido
        </td>
    </tr>
    <tr>
        <td>500000<br>500000</td>
        <td>No hubo ni ganancia ni perdida, la inversion fue un desperdicio de tiempo, pero al menos no de dinero</td>
    </tr>
    <tr>
        <td>
            <input id="input1" value="80000">
            <input id="input2" value="84000">
            <button type="button" id="trigger">Trigger</button>
        </td>
        <td><h4 id="output">Salida</h4></td>
    </tr>
</table>

```python
def porcentaje_ganancia(capital, retorno):
    return ((retorno - capital)/capital)*100

def evaluacion(capital, retorno):
    ganancia = porcentaje_ganancia(capital, retorno)
    if (ganancia > 0):
        return 'Hubo una ganancia de $' + str(retorno - capital) + ' correspondiente al ' + str(ganancia) + ' % del capital invertido'
    elif (ganancia < 0):
        return 'Hubo una perdida de $' + str(capital - retorno) + ' correspondiente al ' + str((ganancia*-1)) + ' % del capital invertido'
    else:
        return 'No hubo ni ganancia ni perdida, la inversion fue un desperdicio de tiempo, pero al menos no de dinero'
```

```python
evaluacion(80000, 84000)
```




    'Hubo una ganancia de $4000 correspondiente al 5.0 % del capital invertido'


