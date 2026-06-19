# Congruencia de Zeller

La congruencia de Zeller es un algoritmo ideado por Julius Christian Johannes Zeller para calcular el día de la semana de cualquier fecha del calendario.

La fórmula es la siguiente, considerando que todas las divisiones son divisiones enteras:

$$[d + \frac{13(m + 1)}{5}+y+\frac{y}{4}-\frac{y}{100}+\frac{y}{400}]mod 7$$

Donde:
- d es el día del mes: 1 a 31
- m es el mes, pero con la siguiente notación: marzo=3, abril=4, ..., enero=13, febrero=14 y es el año, pero si el mes es enero o febrero se disminuye en una unidad mod es el residuo de la división entera (el operador %)

El resultado de aplicar esa fórmula será un valor entre 0 y 6 donde 0 = sábado, 1 = domingo, 2 = lunes, 3 = martes, 4 = miércoles, 5 = jueves, 6 = viernes

### Entrada

La entrada comienza con una línea que contiene la cantidad N de fechas, no más de 1000. Luego siguen N Líneas, cada una fecha en formato (en minúsculas) dia-mes-año

### Salida

La salida tendrá N líneas con el día de la semana correspondiente a cada fecha (sin comillas ni tildes): ‘lunes’, o ‘martes’, o ‘miercoles’, o ‘jueves’, o ‘vieres’, o ‘sabado’, o ‘domingo’

|Ejemplo de entrada |Ejemplo de salida   |
|-------------------|--------------------|
|3                  ||
|25-diciembre-1642  |jueves              |
|20-julio-1969      |domingo             |
|1-enero-2000       |sabado              |

```python

```
