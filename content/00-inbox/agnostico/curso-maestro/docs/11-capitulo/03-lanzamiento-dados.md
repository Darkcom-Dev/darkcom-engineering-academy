# Lanzamiento de dados

Vamos a programar un juego de azar. Uno bastante simple en realidad: la plataforma “arroja” un par de dados y tú haces lo mismo. Quien saque mayor suma gana o, si sacan lo mismo, se declara empate.

Más específicamente, dada una cantidad de turnos, tendrás que simular el resultado de tu lanzamiento (del lanzamiento de la plataforma se encarga obviamente la plataforma).

Pero cuidado, no puedes decir simplemente que es un valor aleatorio entre 2 y 12, sino la suma de dos valores aleatorios entre 1 y 6. Parece lo mismo pero las distribuciones de probabilidad de los resultados no son iguales en los dos casos.

Para entender lo anterior, considera por ejemplo que en un lanzamiento de dos dados la probabilidad de obtener una suma de 2 es de 1/36, mientras que la de obtener 7 es seis veces mayor (1/6). La lista completa de probabilidades se muestra a continuación. Para facilitar su comprensión, se muestra en negro el primer dado y en blanco el segundo.



![image.png](00-inbox/ciencia-de-datos/02-recopilacion-de-datos/02-api/assets/image.png)



### Entrada

La entrada comienza con una línea que contiene la cantidad N de turnos. Luego siguen N líneas con el resultado de los lanzamientos de la plataforma (los cuales obviamente no conocerás antes de enviar tu código). Por cada lanzamiento de la plataforma debes simular el tuyo, pero considerando lo expuesto anteriormente.

### Salida

La salida debe contener N líneas con el mensaje (sin comillas) ‘Gana el humano’, o ‘Gana la plataforma’, o ‘Empate’ según sea el caso.

|Ejemplo de entrada|Ejemplo de salida (suponiendo que tu lanzamiento siempre da 7)|
|------------------|-------------------|
|4  ||
|5  |Gana el humano     |
|11 |Gana la plataforma |
|6  |Gana el humano     |
|7  |Empate             |


```python

```
