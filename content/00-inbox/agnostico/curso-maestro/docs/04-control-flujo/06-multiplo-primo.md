# Múltiplo de cúal primo.

¿Sabes que los cuatro primeros números primos (sin contar al 1) son 2, 3, 5, y 7 verdad? Bueno, pues la idea es que dado un número entero positivo N mayor a 1 se debe decir si es múltiplo de alguno de esos cuatro o si no es múltiplo de ninguno. En caso que sea múltiplo de más de uno de ellos solo se debe mencionar al menor (por ejemplo el 210 es múltiplo de todos, entonces se debería mostrar que es múltiplo de 2).

 En síntesis, se debe mostrar uno, y solo uno, de estos cinco mensajes (sin comillas ni puntuación):
 'N es multiplo de 2'
 'N es multiplo de 3'
 'N es multiplo de 5'
 'N es multiplo de 7'
 'N no es multiplo de ninguno de los primeros cuatro primos'

#### Entrada:
La entrada contiene una única linea con el valor de N.
#### Salida:
Una única salida con el mensaje a mostrar.

| Ej. de entrada | Ej. de salida                                                |
| -------------- | ------------------------------------------------------------ |
| 35             | '35 es múltiplo de 5'                                        |
| 19             | '19 no es múltiplo de ninguno de los primeros cuatro primos' |
| 2              | '2 es múltiplo de 2'                                         |


```python
def multiplo_primo(n):
    multiplos_list = []
    for i in (2,3,5,7):
       if n % i == 0:
           multiplos_list.append(i)
    if len(multiplos_list) == 0:
        return []
    else:
        return multiplos_list[0]

def evaluacion(n):
    multiplo_minimo = multiplo_primo(n)
    
    if len(multiplo_primo(n)) == 0:
        return 'No es multiplo de ninguno de los primeros cuatro primos'
    else:
        return f'{n} Es multiplo de {multiplo_minimo}'
    
```

```python
evaluacion(19)
```


    'No es multiplo de ninguno de los primeros cuatro primos'