# Náufrago

Al mencionar esta famosa película protagonizada por Tom Hanks la imagen que a casi todo el mundo se le viene a la mente es la de su entrañable compañero: Wilson. De lo que muchos no se acuerdan es de la deprimente actividad de ir marcando cada uno de los días en los que estuvo varado en la isla.

Se supone que es casi obligatorio hacerlo si eres un náufrago. E igual de obligatorio es el formato: se hace una raya vertical por cada día pero, cuando se ajustan cinco consecutivos se hace es una raya diagonal sobre las previas cuatro. Eso, obviamente, se hace para que, luego de muchos días, sea más fácil contar por múltiplos de cinco en vez de uno en uno.

Fuente: 
<img src='https://editorial.rottentomatoes.com/article/tom-hanks-s-best-reviewed-movies/2/'>

¿Harías un programa para, dada una fecha de naufragio y otra de rescate, mostrar el conteo correspondiente? Para representar las rayas verticales debes usar ‘1’ y para los bloques de cinco rayas ‘5’. Cada elemento se separa del siguiente por un espacio en blanco.

### Entrada

La entrada comienza con una línea que contiene la cantidad C de casos. Luego siguen C líneas con dos datos separados entre sí por un espacio en blanco: la fecha de naufragio y la fecha de rescate. La segunda siempre será por lo menos un día superior a la primera (si te rescatan el mismo día parece que no se considera naufragio) y ambas estarán en formato dd-mm-aaaa

### Salida

La salida debe contener C líneas, cada una con el conteo correspondiente y debe terminar en 5 o 1, no en espacio en blanco.

|Ejemplo de entrada|Ejemplo de salida|
|------------------|-----------------|
|3|
|01-01-2000 10-01-2000|5 1 1 1 1    |
|01-12-2010 31-12-2010|5 5 5 5 5 5  |
|01-06-2020 04-06-2020|1 1 1        |

```python

```
