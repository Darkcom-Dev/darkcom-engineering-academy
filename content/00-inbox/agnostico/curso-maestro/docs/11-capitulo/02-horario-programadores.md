# Horario de los programadores

Mucho se habla de los programadores y hay numerosos mitos alrededor de ellos. Se dice que son seres taciturnos, que durante la cuarentena de 2020 ni si enteraron que hubo cuarentena, que no se alimentan de comida sino de café únicamente. Hay quienes piensan incluso que vienen de otro planeta o tienen súper poderes.

Uno de esos mitos populares es que parecen vampiros en el sentido que huyen de la luz del sol, trabajando solo a altas horas de la noche y durmiendo en horarios diurnos. Pero como en muchas cosas, la realidad poco tiene que ver con esos mitos populares.

Para poner solo unos ejemplos, en https://ivan.bessarabov.com/blog/famous-programmers-work-time se presenta una recopilación de los hábitos de programación dealgunos de los programadores más famosos del mundo. Para ello, contabilizaron la cantidad de commits realizados en sus respectivos repositorios GIT durante un periodo de tiempo prolongado.

Si no sabes que es un commit o GIT, no te preocupes. GIT es un banco pero de proyectos de programación siendo uno de los más populares https://github.com, mientras que un commit es la acción de guardar o subir archivos a ese repositorio (una actualización de los cambios realizados). En otras palabras, cuando un programador hace un commit generalmente es porque ha terminado de realizar un aporte importante en el código en el que está trabajando.

Como resumen de esa recopilación de hábitos, en la siguiente tabla se muestran la cantidad de commits realizados por franjas horarias

|programador|id     |link|
|-----------|-------|----|
|Linus Torvalds     |1|https://github.com/torvalds/linux|
|Guido van Rossum   |2|https://github.com/python/cpython|

|Franja         |Commits 1|% 1|Commits 2|% 2|
|---------------|-------|-|-------|-|
|00:00 - 03:59  |115    |0.4%   |1130   |10.1%|
|04:00 - 07:59  |1200   |4.3%   |474    |4.2% |
|08:00 - 12:00  |10013  |36.1%  |723    |6.5% |
|12:00 - 15:59  |8520   |30.7%  |2727   |24.4%|
|16:00 - 19:59  |6079   |21.9%  |3304   |29.5%|
|120:00 - 23:59 |1798   |6.5%   |2834   |25.3%|
|Total          |27725  |100.0% |11192  |100.0%|

Notemos que en el caso de Linus Torvals, el padre de Linux, su franja “favorita” está entre 8 de la mañana y 12 del día, mientras que en el caso de Guido van Rossum, el padre de Python, está entre 4 de la tarde y 8 de la noche, nada vampírico, ¿verdad?

Para ayudar a develar esa aura de misterio alrededor de los programadores deberás hacer un programa (oh no, ya te mordieron) para, dada una serie de registros de commits, determinar la cuántos de ellos fueron hechos en cuál franja horaria. Para eso vamos a considerar las siguientes:

- Entre las 00:00 horas y las 03:59 horas, a estos los llamaremos true vampires
- Entre las 04:00 horas y las 07:59 horas, a estos los llamaremos early birds
- Entre las 08:00 horas y las 11:59 horas, a estos los llamaremos sunny warmers
- Entre las 12 :00 horas y las 15:59 horas, a estos los llamaremos lunch workers
- Entre las 16:00 horas y las 19:59 horas, a estos los llamaremos sunset lovers
- Entre las 20:00 horas y las 23:59 horas, a estos los llamaremos prime timers

### Entrada

La entrada comienza con una línea que contiene la cantidad R de registros, hasta 10000.
Luego siguen R líneas, cada una con el registro de un commit en el formato aaaa-mm-dd hh:mm:ss.

### Salida

La salida debe contener seis líneas, cada una con la cantidad de commits en cada franja horaria en el orden y formato que se muestra en el ejemplo.

| Ejemplo de entrada | Ejemplo de salida |                 |
| ------------------ | ----------------- | --------------- |
| 7                  |                   |                 |
| 2000-01-01         | 14:30:00          |                 |
| 2000-01-02         | 01:23:45          | true vampires 1 |
| 2000-01-03         | 18:18:18          | early birds 0   |
| 2000-01-04         | 20:20:20          | sunny warmers 1 |
| 2000-01-05         | 09:09:09          | lunch workers 2 |
| 2000-01-06         | 22:22:22          | sunset lovers 1 |
| 2000-01-07         | 15:45:00          | prime timers 2  |

```python

```
