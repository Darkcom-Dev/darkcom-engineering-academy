# Cadena de palabras

Dado una lista de palabras, decimos que estas están en cadena si la parte final de la primera coincide con la inicial de la segunda, la parte final de la segunda coincide con la inicial de la tercera, y así sucesivamente hasta el final.

Un ejemplo de cadena (sin considerar acentos) es: coma manzana naranja jamas masa asada

Haciendo la simplificación que todas las palabras tendrán por lo menos cuatro letras y que por “parte final” o “parte inicial” nos referimos a las últimas dos o tres letras, ¿podrías hacer un programa para verificar si una lista de palabras está o no en cadena? Ten en cuenta que al leer la lista está no necesariamente tendrá sentido.

### Entrada

La entrada consiste en un conjunto de listas de palabras, separadas entre sí por un espacio en blanco y de a una lista por línea, el cual se encontrará en el archivo cadena.txt. 

Cada lista tendrá por lo menos dos palabras.

### Salida

Por cada lista de palabras, la salida tendrá una línea con el mensaje (sin comillas) ‘cadena completa’ o ‘cadena rota’ según sea el caso:

|Ejemplo de entrada|Ejemplo de salida|
|------------------|-----------------|
|gato toma mapa|cadena completa|
|padre madre|cadena rota|
|raza zapato toronja jazmin minuta tapa aparato|cadena completa|
|pedro dron naranja|cadena rota|

```python

```
