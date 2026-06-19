# Calificación del curso

En un curso de programación, no diremos en cuál, el profesor hace 12 talleres, con 9, 11, 12, 8, 12, 9, 11, 8, 11, 10, 9, y 10 ejercicios respectivamente. La calificación del curso se calcula de la siguiente manera. Cada taller tiene una calificación individual que es igual a la cantidad de ejercicios resueltos correctamente sobre la cantidad de ejercicios del taller. Eso multiplica por 5 y se redondea a una cifra decimal. Luego se toman esas 12 calificaciones, se promedian y se redondea de nuevo.

Dado el registro de la cantidad de ejercicios resueltos en cada taller por N estudiantes, así como su correspondiente documento de identidad ¿harías un programa para calcular la nota definitiva y mostrar el resultado ordenado de manera ascendente según el documento?

Para ello ten recuerda que primero se calcula la calificación de cada taller, se redondea, luego se calcula la calificación final y se redondea de nuevo, en ambos casos a una cifra decimal. Si solo haces uno de los redondeos, es posible que los resultados no coincidan exactamente. También ten en cuenta que debes considerar el documento del estudiante como número no como texto, para que el ordenamiento coincida.

### Entrada

La entrada comienza con una línea que contiene la cantidad N de estudiantes. Luego siguen N líneas, cada una con los datos separados entre sí por una coma y un espacio en blanco: el documento de identidad y la cantidad de ejercicios resueltos por cada estudiante en los 12 talleres (en el orden correspondiente).

### Salida

La salida contener N líneas, cada una con el documento y la calificación final, separados entre sí por un espacio en blanco y ordenados ascendentemente por documento.

|Ejemplo de entrada|Ejemplo de salida|
|------------------|-----------------|
|4||
|888888,8,8, 8, 8, 8, 8,8,8, 8, 8, 8, 8         |111111 5.0|
|555555,5,5, 5, 5, 5, 5,5,5, 5, 5, 5, 5         |555555 2.6|
|777777,7,7, 7, 7, 7, 7,7,7, 7, 7, 7, 7         |777777 3.6|
|111111,9,11, 12, 8, 12,9,11, 8, 11, 10, 9, 10  |888888 4.0|

```python

```
