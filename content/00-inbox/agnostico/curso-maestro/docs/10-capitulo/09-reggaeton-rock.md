# Reggaetón vs Rock

Es usual, aunque como en todo hay excepciones, que los fanáticos de uno de estos géneros no gusten del otro. Hay muchos argumentos por un lado y por el otro, y para “echarle más leña al fuego”, vamos a usar nuestras habilidades de programación para ahondar en uno de ellos: la riqueza léxica, la cual consiste en determinar la cantidad de vocablos o unidades léxicas diferentes (palabras) que un texto contiene. En este caso los textos a los que haremos referencia serán las letras de las canciones.

Para ser lo más objetivos posibles, vamos a hacer los siguiente:
- Consideraremos un único idioma: el español
- Tomaremos como referentes a artistas reconocidos en ambos géneros
- Usaremos un mismo tema, bastante popular por cierto: el despecho o desamor

### Entrada

La entrada comienza con una línea que contiene la cantidad N de líneas de la canción de Reggaeton, no más de 1000, seguida por N líneas con los fragmentos de la misma. Luego sigue una línea que contiene la cantidad M de líneas de la canción de Rock, tampoco más de 1000, seguida por M líneas con los fragmentos de la misma. Por simplicidad en los fragmentos de las canciones no habrá mayúsculas, ni tildes, ni signos de puntuación.

### Salida

La salida tendrá una única línea con el mensaje (sin comillas ni tildes): ‘Reggaeton: X Rock: Y’ siendo X y Y la cantidad de palabras diferentes en cada canción.

**Ejemplo de entrada** (Tusa de Karol G. y Niki Minaj versus Crimen de Gustavo Cerati)
```
8
ya no tiene excusa
hoy salio con su amiga
dizque pa matar la tusa
que porque un hombre
le pago mal
esta dura y abusa
se canso de ser buena
ahora es ella quien los usa

9
la espera me agoto
no se nada de vos
dejaste tanto en mi
en llamas me acoste
y tras un lento degrade
supe que te perdi
que otra cosa puedo hacer
si no olvido morire
y otro crimen quedara sin resolver
```

**Ejemplo de salida**

Reggaeton: 36 Rock: 36

```python

```
