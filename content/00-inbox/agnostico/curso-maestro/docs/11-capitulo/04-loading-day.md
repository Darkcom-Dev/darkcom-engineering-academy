# Loading day

Melisa es programadora y en su habitación tiene el reloj de pared más cool del mundo.

En vez de decir la hora, tiene una pantalla digital retro que muestra el mensaje “Loading day ...” y luego el porcentaje del día transcurrido, con hasta tres cifras decimales.

Así por ejemplo, cuando es medio día dice “Loading day ... 50.0%”, cuando son las nueve de la noche dice “Loading day ... 87.5%”, cuando falta un segundo para media noche “Loading day ... 99.999%” y así sucesivamente.

Hasta la cantidad de cifras de decimales es una genialidad, considerando que un día tiene $24*60*60 = 86400$ segundos.

¿Podrías hacer un programa para, dada una hora en formato hh:mm:ss AM/PM simular lo que mostraría la pantalla de ese reloj?

### Entrada

La entrada comienza con una línea que contiene la cantidad C de casos. Luego siguen C líneas cada una con una hora del día en el formato indicado.

### Salida

La salida debe contener C líneas, cada una con el mensaje correspondiente.

| Ej. de entrada (coinciden con los anteriores) | Ej. de salida           |
| --------------------------------------------- | ----------------------- |
| 3                                             |                         |
| 12:00:00 PM                                   | Loading day ... 50.0%   |
| 09:00:00 PM                                   | Loading day ... 87.5%   |
| 11:59:59 PM                                   | Loading day ... 99.999% |

```python

```
