# Manchas solares

Una mancha solar es una región del Sol que tiene una temperatura más baja que sus alrededores, y con una intensa actividad magnética.

Se componen típicamente de una región central oscura, llamada “umbra”, rodeada por una “penumbra” más clara. Una sola mancha puede llegar a medir lo mismo el diámetro de la Tierra, es decir cerca a doce mil kms pero pueden llegar a ser incluso más grandes.

Las primeras observaciones sistemáticas de manchas solares fueron hechas por astrónomos chinos a partir del 28 A.C.

Se presume que podían verlas cuando la intensa luz del sol era filtrada por el polvo que el viento llevaba desde los desiertos del Asia central.

En Occidente, la noticia más antigua sobre una mancha solar aparece en “la Vida de Carlomagno”, escrita en 807 D.C.

En los siglos siguientes las observaron astrónomos musulmanes como Averroes y, ya en el siglo XV, también italianos.

En 1610 los astrónomos David Fabricius y su hijo Johannes observaron manchas mediante telescopios. David publicó una descripción en junio de 1611, fecha cercana a la cual Galileo Galilei enseñó manchas solares a astrónomos en Roma.

El número de manchas solares ha sido medido desde 1700 D.C como se muestra en el siguiente gráfico y donde puede dichos fenómenos están relacionados con la actividad solar que ocurre en ciclos de aproximadamente once años.

Fuente: 
<img src='https://upload.wikimedia.org/wikipedia/commons/0/07/Ssn_yearly.jpg'>

Dado un registro de las fechas exactas de aparición de manchas solares, ¿podrías determinar cuánto tiempo transcurre entre una y otra, así como el tiempo promedio?

### Entrada

La entrada comienza con una línea que contiene la cantidad R de registros (2 ≤ R ≤ 5000).

Luego siguen R líneas, cada una con la fecha de aparición en formato `aaaa-mm-dd hh:mm:ss` donde las horas están entre 0 y 23. Estas fechas están ordenadas cronológicamente.

### Salida

La salida comienza con R-1 líneas, cada una con el tiempo transcurrido entre una fecha y la siguiente, todas con el formato (sin tildes) X dias, Y horas, Z minutos, W segundos.

Las fracciones de segundo no se consideran. Luego sigue una línea en blanco y luego una última línea con el tiempo promedio transcurrido entre todas las fechas en el mismo formato de las primeras R-1 líneas y precedido del mensaje (sin comillas) ‘Promedio: ’
(ver ejemplo)

|Ejemplo de entrada|Ejemplo de salida|
|------------------|-----------------|
|5||
|2000-01-03 01:10:00||
|2000-01-04 06:10:15|1 dias,5 horas,0 minutos, 15 segundos |
|2000-01-08 13:20:20|4 dias,7 horas,10 minutos, 5 segundos |
|2000-01-13 19:40:45|5 dias,6 horas,20 minutos, 25 segundos|
|2000-01-15 23:55:59|2 dias,4 horas,15 minutos, 14 segundos|

Promedio: 3 dias, 5 horas, 41 minutos, 29 segundos

```python

```
