# La espera desespera

Karen no entiende por qué su novio se demora tanto cuando va a la barbería. “Casi ni pelo tiene y de barba tiene más mi sobrino de 13 años”, se dice para sus adentros. Lo que ella no sabe es que en la barbería hay consolas de juegos y bebidas, por lo que su novio adora pasar el tiempo allá.

Dada una serie de registros de la actividad del novio, ¿ayudarías a Karen a hacerle ver todo el tiempo que pasa allá?

### Entrada

La entrada comienza con una línea que contiene la cantidad R de registros. Luego siguen R líneas con los siguientes datos: la fecha (en formato aaaa-mm-dd), el nombre de la actividad, la hora de salida, y la hora de entrada, (ambas en formato “militar” hh:mm:ss).

Los datos están separados entre sí por una coma y un espacio en blanco. En el caso de las actividades, estas por simplicidad siempre estarán en minúsculas y no contendrán tildes ni signos de puntuación. Ir a la barbería siempre se marcará como ‘barberia’. Toda actividad se lleva a cabo dentro del mismo día y las actividades no se cruzan entre sí.

### Salida

La salida debe contener dos líneas. La primera con la cantidad de veces que el novio fue a la barbería, y la segunda con la duración promedio de cada visita en formato horas, minutos, segundos (el formato exacto se muestra en el ejemplo a continuación y se debe considerar que las fracciones de segundo no se consideran).

**Ejemplo de entrada**

- 6
- 2000-06-10, barberia, 14:00:00, 18:30:00
- 2000-06-12, supermercado, 08:00:00, 12:00:00
- 2000-06-20, odontologia, 16:20:00, 17:35:00
- 2000-06-25, barberia, 09:45:00, 12:10:40
- 2000-07-08, barberia, 15:33:20, 20:00:55
- 2000-7-13, supermercado, 12:34:56, 14:54:32

**Ejemplo de salida**
- 3 veces
- 3 horas, 47 minutos, 45 segundos

```python

```
