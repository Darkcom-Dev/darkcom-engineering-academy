# Mal corredor.

Tengo un amigo que corre maratones, pero tengo la sospecha que no es muy bueno.

Cuando le pregunto cómo le fue en una carrera nunca me dice en que puesto queda, y para colmo de males al decirme el tiempo que se demoró lo hace en segundos en vez de hacerlo en horas:minutos:segundos como todo el mundo. Yo creo que es para que no me
dé cuenta que se demoró mucho.

¿Podrías hacer un programa para llevar a cabo la conversión?
#### Entrada:
La entrada contiene una única línea con el tiempo en segundos.
#### Salida:
El valor correspondiente en formato horas:minutos:segundos. Ten en cuenta que no es
necesario anteceder 0 cuando alguno de los tres elementos es menor que 10, es decir,
una salida como 2:9:7 es correcta y no se debe convertir a 2:09:07.


| Ej. de entrada | Ej. de salida |
| -------------- | ------------- |
| 19788          | 5:29:48       |


```python
def conversor_segundos_a_hora(segundos):
    horas = segundos // 3600
    segundos_restantes = segundos % 3600
    minutos = segundos_restantes // 60
    segundos = segundos_restantes % 60
    return "{}:{}:{}".format(horas, minutos, segundos)
```

```python
conversor_segundos_a_hora(19788)
```


    '5:29:48'


