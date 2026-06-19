# Auténtico calendario colombiano

Todo el que tenga un teléfono móvil, incluso sino es muy “inteligente” tiene en su mano un calendario. Sin embargo hay quienes, sea por nostalgia o por otras razones, prefieren tener un calendario físico (en papel) sea de los de poner en una mesa o los de colgar en pared.

La mayoría de ellos tienen alguna imagen de fondo, casi siempre la publicidad de algún producto o negocio, pero el formato del calendario como tal no varía mucho: por cada mes se muestra una tabla donde los encabezados de las columnas son los días de la semana: lunes, martes, etc. y donde en las filas se muestra el día del mes.

Así por ejemplo, enero de 2000, se muestra así:

|lun |mar |mie |jue |vie |sab |dom |
|----|----|----|----|----|----|----|
|    |    |    |    |    |1   |2   |
|3   |4   |5   |6   |7   |8   |9   |
|10  |11  |12  |13  |14  |15  |16  |
|17  |18  |19  |20  |21  |22  |23  |
|24  |25  |26  |27  |28  |29  |30  |
|31  |    |    |    |    |    |    |

Dada una fecha en formato `dd/mm/aaaa` ¿harías un programa para “dibujar” el calendario del mes correspondiente? No es necesario incluir el borde de la tabla, pero los datos si deben aparecer separados entre sí por una tabulación (\t) para que queden organizados como en una. Ten cuidado, los encabezados (nombres de los días) también deben estar tabulados y no debe haber tabulaciones “sobrantes” en las márgenes derechas. En el caso de la semana no empiece un lunes deberán aparecer las tabulaciones correspondientes pero no espacios en blanco.

### Entrada

La entrada comienza con una línea que contiene la cantidad C de casos, no más de 50.
Luego siguen C líneas, cada una con una fecha en formato dd/mm/aaaa.

### Salida

La salida debe “dibujar” cada calendario según las indicaciones dejando un renglón en blanco entre uno y otro. Al final del último calendario también debe haber un renglón en blanco.

**Ejemplo de entrada**

- 3
- 10/02/2010
- 05/01/2000
- 29/02/1996

**Ejemplo de salida**

|lun |mar |mie |jue |vie |sab |dom |
|----|----|----|----|----|----|----|
|1   |2   |3   |4   |5   |6   |7   |
|8   |9   |10  |11  |12  |13  |14  |
|15  |16  |17  |18  |19  |20  |21  |
|22  |23  |24  |25  |26  |27  |28  |

|lun |mar |mie |jue |vie |sab |dom |
|----|----|----|----|----|----|----|
|----|----|----|----|----|1   |2   |
|3   |4   |5   |6   |7   |8   |9   |
|10  |11  |12  |13  |14  |15  |16  |
|17  |18  |19  |20  |21  |22  |23  |
|24  |25  |26  |27  |28  |29  |30  |
|31  |

|lun |mar |mie |jue |vie |sab |dom |
|----|----|----|----|----|----|----|
|----|----|----|1   |2   |3   |4   |
|5   |6   |7   |8   |9   |10  |11  |
|12  |13  |14  |15  |16  |17  |18  |
|19  |20  |21  |22  |23  |24  |25  |
|26  |27  |28  |29  |


```python

```
