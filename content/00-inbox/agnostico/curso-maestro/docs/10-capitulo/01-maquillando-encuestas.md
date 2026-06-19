# Maquillando la encuesta

![image.png](00-inbox/ciencia-de-datos/02-recopilacion-de-datos/02-api/assets/image.png)
<img src='https://masalladelarisa.wordpress.com/2013/01/26/corrupcion-springfiel/'>


Para mejorar su imagen, empañada por flagrantes actos de incompetencia y corrupción, el alcalde diamante gastó una fortuna (de recursos públicos obviamente) en campañas publicitarias. Posterior a ello, mandó a realizar una encuesta a todos los ciudadanos en la que se les preguntó su sexo, edad, su percepción sobre el alcalde (positiva o negativa), y qué tanto les gustaba la pizza en una escala de 1 a 10 siendo, pregunta por demás extraña en una encuesta de tinte político. Además ¿a quién no le gusta la pizza?

Una vez se tuvieron los resultados, sacó un comunicado de prensa con ellos, pero no sin antes “maquillarlos un poco”. Mostró solo aquellas respuestas en las que la percepción fuera positiva (poniéndolas además en mayúsculas para que tuvieran mayor impacto visual) y las ordenó de manera descendente según el gusto por la pizza. Cuando había más de un resultado con el mismo gusto por la pizza, los ordenó por la edad, también de manera descendente. El comunicado decía más o menos así:

Agradecemos el apoyo de los ciudadanos. A continuación, mostramos los resultados de la encuesta realizada en días pasados donde se encuentra la edad, percepción sobre el alcalde, y calificación general de nuestra gestión.

### Entrada

La entrada comienza con una línea que contiene la cantidad N de ciudadanos que contestaron la encuesta, no más de 1000. Luego siguen N líneas, cada una con los cuatro datos separados entre sí por un espacio en blanco: sexo (M o F), edad (≥18), percepción sobre el alcalde (positiva o negativa), y gusto por la pizza (1 a 10).

### Salida

La salida debe contener los resultados “maquillados”.

|Ejemplo de entrada|Ejemplo de salida|
|------------------|-----------------|
|6||
|M 45 positiva 7    ||
|F 22 negativa 6    |30 POSITIVA 10|
|M 19 negativa 9    ||
|F 24 positiva 10   |24 POSITIVA 10|
|F 27 negativa 9    ||
|M 30 positiva 10   |45 POSITIVA 7|

```python

```
