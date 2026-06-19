# Asesinos seriales periódicos

¿Quién no ha visto esas películas o series de suspenso policiacas en las que un detective (hombre o mujer) debe atrapar a un asesino serial?

Recordemos que un asesino serial es quien comete el crimen más de una vez y generalmente lo hace con el mismo modus operandi.

<img src='https://pixy.org/1128271/'>
Fuente: https://pixy.org/1128271/


Pues ahora vamos a suponer que haces parte del equipo de investigación y, con tus habilidades de programación, vas a poder ayudar a resolver algunos casos. Para ello tendrás a tu disposición los registros de crímenes de los últimos 50 años.

De esas series y películas has aprendido que muchos de esos criminales en serie no solo repiten su modus operandi, sino que también cometen los crímenes con una periodicidad bien definida: cada luna llena, cada 1000 días, cosas por ese estilo. A los que tienen esa particularidad los llamaremos asesinos seriales periódicos. Pero cuidado, no deben confundirse con aquellos que atacan en fechas específicas (esos son fáciles de detectar hasta para los investigadores no programadores), pues esos no necesariamente serán periódicos en el sentido de la cantidad de días transcurridos entre un crimen y otro debido la diferencia de días entre meses o a la de días entre años en caso de los bisiestos.

Siendo así, deberás determinar cuáles de los criminales de los que tienes registros son de periódicos “estrictos” y, esto es lo más importante, determinar cuál será la fecha exacta de su próximo crimen.

### Entrada

La entrada comienza con una línea que contiene la cantidad C de casos por revisar (por lo menos uno). Cada caso comienza con una línea que contiene dos datos separados entre sí por un espacio en blanco: el alias del criminal y la cantidad R de registros que se tiene de él o ella (por lo menos tres y máximo mil) y. Por simplicidad, ese alias estará en minúsculas y no tendrá tildes. Luego de esa línea siguen R más, cada una la fecha en formato aaaa-mm-dd del crimen. Esas fechas estarán en orden cronológico.

### Salida

Por cada caso, la salida debe contener uno de los siguientes dos mensajes (sin comillas ni tildes), de a un mensaje por línea y separados entre sí por una línea en blanco. Al final de la salida también deberá quedar una línea en blanco:

- ‘X ataca cada Y dias y volvera a hacerlo en Z’, siendo X el alias del criminal, Y la cantidad de dias exactos entre los crímenres y Z la fecha en formato aaaa-mm-dd del próximo crimen; o
- ‘X no es asesino(a) serial periodico’

**Ejemplo de entrada**
```py
4
el comediante, 3
2000-01-01
2000-01-15
2000-01-29
la amante de gatos, 4
1998-12-12
1999-12-12
2000-12-12
2001-12-12
el asesino de la calle 9, 5
2009-09-09
2012-06-04
2015-02-28
2017-11-23
2020-08-18
el estrangulador de la peluca de payazo, 4
2005-07-10
2005-08-10
2005-09-10
2005-10-10
```

**Ejemplo de salida**
- el comediante ataca cada 14 dias y volvera a hacerlo en 2000-02-12
- la amante de gatos no es asesino(a) serial periodico el asesino de la calle 9 ataca cada 999 dias y volvera a hacerlo en 2023-05-14
- el estrangulador de la peluca de payazo no es asesino(a) serial periodico

```python

```
