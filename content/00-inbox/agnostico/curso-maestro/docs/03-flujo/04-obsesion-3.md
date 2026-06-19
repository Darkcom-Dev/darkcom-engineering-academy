# Obsesión con el número 3

El problema es el siguiente: Carolina está obsesionada con el número 3, hasta el punto que lo ve por todas partes. Y cuando no lo ve directamente, ve sus múltiplos, y cuando no ve sus múltiplos ve los números más cercanos que son múltiplos.

Así por ejemplo, 
- si ve un 3 dirá (sin comillas ni puntuación): 'El numero 3 es el mejor' 
- Si ve un numero N que es multiplo de 3 dirá (sin comillas ni puntuación): 'El numero N es multiplo de 3. Y el numero 3 es el mejor'
- Si ve un numero N que no es múltiplo de 3 dirá una de dos cosas (sin comillas ni puntuación): 'El numero N no es múltiplo de 3, pero si le restas 1 el resultado si lo es. Y el numero 3 es el mejor' o 'El numero N no es múltiplo de 3, pero si le sumas 1 el resultado si lo es. Y el numero 3 es el mejor'.

#### Entrada:
La entrada contiene una única línea con el valor de N que corresponde a un número
entero positivo.
#### Salida: 
Una única línea con el mensaje a mostrar.

<table>
    <tr>
        <th>Ejemplos de entrada</th>
        <th>Ejemplos de salida</th>
    </tr>
    <tr>
        <td>26</td>
        <td>El numero 26 no es multiplo de 3, pero si le sumas 1 el resultado si lo es. Y el numero 3 es el mejor</td>
    </tr>
    <tr>
        <td>3</td>
        <td>El numero 3 es el mejor</td>
    </tr>
    <tr>
        <td>12</td>
        <td>El numero 12 es multiplo de 3. Y el numero 3 es el mejor</td>
    </tr>
    <tr>
        <td>25</td>
        <td>El numero 25 no es multiplo de 3, pero si le restas 1 el resultado si lo es. Y elnumero 3 es el mejor.</td>
    </tr>
    
</table>

```python
def obsesion(numero):
    if numero == 3:
        return 'El numero 3 es el mejor'
    elif numero % 3 == 0:
        return 'El numero ' + str(numero) + ' es multiplo de 3. Y el numero 3 es el mejor'
    elif numero % 2 == 0:
        return 'El numero ' + str(numero) + ' no es multiplo de 3, pero si le sumas 1 el resultado si lo es. Y el numero 3 es el mejor'
    else:
        return 'El numero ' + str(numero) + ' no es multiplo de 3, pero si le restas 1 el resultado si lo es. Y el numero 3 es el mejor'
```

```python
# Test
obsesion(26)
```




    'El numero 26 no es multiplo de 3, pero si le sumas 1 el resultado si lo es. Y el numero 3 es el mejor'


