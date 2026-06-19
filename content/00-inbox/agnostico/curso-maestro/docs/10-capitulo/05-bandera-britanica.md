# Bandera británica

Miguel ya pasó por la bandera inglesa, luego la escocesa, es apenas natural que siguiera con la británica solo que, en vez de dibujarla, quiere comprobar si un determinado dibujo corresponde a ella o no.

Por simplicidad, los dibujos estarán compuestos únicamente por los caracteres ‘0’, ‘+’, y ‘#’, y solo se consideran banderas de tamaño LxL (cantidad de caracteres horizontales y verticales) siendo L un valor impar en el rango [5, 25].

Así por ejemplo, una bandera de dimensión 7x7 se verá de la siguiente manera:

```bsh
#00+00#
0#0+0#0
00#+#00
+++#+++
00#+#00
0#0+0#0
#00+00#
```

¿Harías un programa como el de Miguel?

### Entrada

La entrada comienza con una línea que contiene la cantidad C de casos, no más de 50.
Cada caso comienza con una línea que contiene el valor de L, seguida por L líneas con el dibujo correspondiente (cada línea con L caracteres).

### Salida

Por cada caso de prueba la salida debe contener una línea con el mensaje (sin comillas ni acento) ‘Bandera britanica’ o ‘Ni idea’ según sea el caso.

**Ejemplo de entrada**

3

5
```
#0+0#
0#+#0
++#++
0#+#0
#0+0#
```
5
```
#####
#+++#
#+#+#
#+++#
#####
```
7
```
#00+00#
0#0+0#0
00#+#00
+++#+++
00#+#00
0#0+0#0
#00+00#
```
**Ejemplo de salida**

- Bandera britanica
- Ni idea
- Bandera britanica

```python

```
