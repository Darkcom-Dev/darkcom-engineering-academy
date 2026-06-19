# Top insalubre

Esta es una historia real, no vayan a pensar que se trata solo de terminar la trilogía de ejercicios de personajes secundarios de los Simpsons. En una determinada empresa, muy preocupados por el estado de salud de sus empleados, decidieron llevar a cabo una campaña para mejorar los hábitos alimenticios y deportivos.

Fuente: 
<img src='https://frinkiac.com/caption/S07E15/134901'>

Cada semana, a cada empleado se le toman las siguientes medidas: peso (en kgs), estatura (en mts), azúcar en sangre (en mg/dL), y triglicéridos (en mg/dL).

Al calcular el índice de masa corporal, o IMC, como peso / estatura 2 redondeado a una cifra decimal, si este es mayor a 25, y el nivel de azúcar en sangre es superior a 100 y el nivel de triglicéridos es superior a 150, ese empleado se considera en riesgo y se le hace un acompañamiento individual.

Hasta allí, todo muy bien. El problema es que cada semana la empresa publica en el boletín de forma masiva los resultados en lo que los empleados pusieron el nombre de “top insalubre” donde salen esos empleados en riesgo en orden descendente según el IMC.

Nadie quiere hacer parte del top insalubre, y mucho menos estar en los primeros lugares, lo cual se ha vuelto una fuente de estrés que, irónicamente, hace que muchos empleados tengan malos hábitos de alimentación.

### Entrada

La entrada comienza con una línea que contiene la cantidad E de empleados. Luego siguen E líneas, cada una con los siguientes datos de cada empleado separados entre sí por una coma y un espacio en blanco: nombre completo, peso, estatura, nivel de azúcar, nivel de triglicéridos.

### Salida

La salida debe contener el top insalubre (siempre habrá por lo menos un empleado en riesgo) en el que se muestra, de a uno por línea, los siguientes datos: puesto en el top, nombre completo e IMC. El puesto en el top está dado por el valor del IMC ordenado descendentemente. Recuerda que ese valor debió haberse redondeado previamente. En caso de empate por IMC, el desempate será por nombre, también de manera descendente.


|Ejemplo de entrada                         |Ejemplo de salida           |
|-|-|
|5                                          ||
|Pedro Perez, 70.0, 1.78, 80.0, 132.0       ||
|Fernanda Fernandez, 70, 1.58, 110.0, 160.0 ||
|Rodrigo Rodriguez, 99.0, 1.75, 150.0, 240.0|1 Rodrigo Rodriguez 32.3    |
|Jimena Jimenez, 56.0, 1.72, 95.0, 124.0    |2 Gonzalo Gonzalez 28.0     |
|Gonzalo Gonzalez, 81, 1.70, 115.0, 165.0   |3 Fernanda Fernandez 28.0   |

```python

```
