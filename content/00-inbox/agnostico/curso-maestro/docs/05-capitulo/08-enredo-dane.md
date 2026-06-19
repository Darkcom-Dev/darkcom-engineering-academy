# Enredo en el DANE

Supongamos que, en algún momento de la historia, el Departamento Administrativo Nacional de Estadística (DANE) mantenía el registro diario de la cantidad promedio de nacimientos desde el inicio del año hasta cada fecha.

Aunque a primera vista parece una buena manera de almacenar la información, cada que se necesitaba saber la cantidad exacta de nacimientos en un día especifico, algún desafortunado empleado tenía que ponerse a hacer cuentas para conocerla.

¿Le ayudarías al DANE haciendo un programa para que, dados esos valores diarios de los promedios se muestre la cantidad diaria de nacimientos?

Así por ejemplo si se tiene que la cantidad promedio de nacimientos para los cinco primeros días del año fue: 10, 12, 12, 13, y 12.6 fue porque la cantidad diaria de nacimientos para esas fechas fue de 10, 14, 12, 16 y 11.

#### Entrada
La entrada comienza con una línea que contiene un valor entero N que corresponde a la cantidad de días de los que se tienen registros (siempre se empieza el primero de enero y no serán más de 365). Luego siguen N líneas con cada uno de los promedios diarios.

Se garantiza que dichos valores son tales que la cantidad diaria de nacimientos son valores enteros.

#### Salida
La salida contiene N líneas con los valores enteros positivos que corresponden a la cantidad diaria de nacimientos.

| Ej. de entrada | Ej. de salida |
| -------------- | ------------- |
| 6              | 20            |
| 20.0           | 23            |
| 21.5           | 20            |
| 21.0           | 24            |
| 21.75          | 25            |
| 22.4           | 23            |
| 22.5           |               |

