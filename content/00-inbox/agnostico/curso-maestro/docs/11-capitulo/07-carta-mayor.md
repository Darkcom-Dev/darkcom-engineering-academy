# Carta mayor

Vamos a programar un juego de azar con la baraja de cartas inglesa. Recordemos que dicha baraja contiene 52 cartas, distribuidas en 4 palos diferentes: picas (♠), diamantes (♦), tréboles (♣) y corazones (♥). De cada uno, hay trece cartas, con valores del 2 hasta el 10, más cuatro figuras, Jack (J), Queen (Q), King (K), y As (A), que para este juego corresponderán numéricamente a los valores 11, 12, 13 y 1.

Sin considerar los palos, se repartirán aleatoria-mente 13 cartas para ti, y 13 para la plataforma. Cada uno tendrá las cartas apiladas “hacia abajo”, es decir, no sabrá cuales le tocaron ni podrá reorganizarlas. Al mismo tiempo, tú y la plataforma destaparán la carta que esté arriba de su respectiva pila. Quien tenga el mayor valor numérico ganará un punto en ese turno, mientras que en caso de empate no habrá puntos en ese turno. Las cartas “destapadas” no vuelven a la pila. Al terminar los 13 turnos, quien tenga más puntos, ganará la partida.

Deberás simular varias partidas. En cada una tú te encargarás de repartir tus 13 cartas aleatoria-mente, (las 13 de la plataforma se eligen después de las tuyas pero de este proceso no debes preocuparte).

### Entrada

La entrada comienza con una línea que contiene la cantidad P de partidas. Luego siguen P líneas con las 13 cartas que le tocaron a la plataforma (la cual obviamente no conocerás antes de enviar tu código) y separadas entre sí por un espacio en blanco. El palo (pica o corazón) no tiene relevancia en el juego por lo que solo aparecerá el valor numérico de cada carta. Recuerda que en cada partida, e independiente de las cartas que haya recibido la plataforma, tú deberás simular tus 13.

### Salida

La salida debe contener P líneas, cada una con el mensaje (sin comillas) ‘Puntos humano:
X Puntos plataforma: Y’ de cada uno de las partidas.

|Ejemplo de entrada|Ejemplo de salida (suponiendo que tu pila siempre será 7 2 3 4 5 6 7 8 9 10 11 12 10)|
|-|-|
|3||
|13 1 2 3 4 5 6 1 8 9 13 11 12|Puntos humano: 10 Puntos plataforma: 3|
|1 2 3 4 5 6 1 9 11 12 13 13 8|Puntos humano: 3 Puntos plataforma: 5 |
|13 13 12 11 9 8 6 5 4 3 2 1 1|Puntos humano: 7 Puntos plataforma: 6 |

```python

```
