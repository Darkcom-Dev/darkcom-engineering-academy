# Quédate el cambio
 
Cuando uno es niño(a) y lo(a) mandaban a la tienda a comprar cosas, la frase esperada era “y te puedes quedar con el cambio” (lo que dependiendo del país/región también se le dice “la devuelta” o “los vueltos”, entre otras denominaciones). Bueno, pues resulta que en la casa de Daniel al parecer son bastante tacaños puesto que la única manera en que Daniel se puede quedar con el cambio es cuando este es múltiplo de 10 o de 15, pero no de 4.

Así por ejemplo si total de lo que va a comprar es $8000 y lo mandan con un billete de $10000 lamentablemente no se puede quedar con el cambio de $2000 pues, aunque este es múltiplo de 10, también es múltiplo de 4.

#### Entrada:
La entrada contiene dos líneas. La primera contiene un valor entero positivo que corresponde al valor total de la compra (V), mientras que la segunda contiene un valor también entero positivo que corresponde al efectivo que se lleva para pagar (E). Siempre se cumple que E ≥ V.
#### Salida:
Una o dos líneas. La primera con el valor del cambio y, opcionalmente la segunda con el mensaje (sin comillas) 'y te lo puedes quedar'.

| Ej. de entrada | Ej. de salida              |
| -------------- | -------------------------- |
| 12560 -> 20000 | 7440                       |
| 13950 -> 15000 | 1050 y te lo puedes quedar |
| 5000 -> 5000   | 0                          |
| 1235 -> 2000   | 765 y te lo puedes quedar  |


```python
def quedate_cambios(v, e):
    cambio = e - v
    if cambio % 4 == 0 and cambio < v:
        return cambio
    elif cambio % 10 == 0 or cambio % 15 == 0:
        return f'{cambio} y te lo puedes quedar'
    else:
        return 'los vueltos'
```

```python
quedate_cambios(12560, 20000)
quedate_cambios(13950, 15000)
quedate_cambios(5000, 5000)
quedate_cambios(1235, 2000)
```


    '765 y te lo puedes quedar'


