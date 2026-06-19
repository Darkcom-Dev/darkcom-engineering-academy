# Color o escaleras

Para quienes no saben, el póquer es un juego que mezcla suerte y astucia, y que se juega con la baraja de cartas inglesa. Dicha baraja contiene 52 cartas, distribuidas en 4 palos diferentes:

- picas (♠)
- diamantes (♦)
- tréboles (♣)
- corazones (♥). 
 
De cada uno, hay trece cartas, con valores del 2 hasta el 10, más las cuatro figuras:

- Jack (J)
- Queen (Q)
- King (K)
- As (A), 

que numéricamente corresponden a los valores 11, 12, 13 y 14.

Aunque hay diversas variaciones como draw poker, Omaha hold 'em, Texas hold 'em, entre otras, la idea general es obtener (o hacer pensar a los otros jugadores que lo hiciste), la mejor mano con cinco cartas. Las manos posibles se presentan a continuación.


![image.png](00-inbox/ciencia-de-datos/02-recopilacion-de-datos/02-api/assets/image.png)
Fuente: https://es.wikipedia.org/wiki/P%C3%B3quer


Concentrándonos en cuatro de esas diez posibles manos, el problema consiste en, dadas las cinco cartas de un jugador, determinar si ellas corresponden a una Escalera “normal” (top 6), a un Color (top 5), a una Escalera de color (top 2), a una Escalera real (top 1), o a ninguna de ellas.

### Entrada

La entrada comienza con una línea que contiene la cantidad M de manos a evaluar, por cada mano seguirán diez líneas con la información de las cinco cartas. Cada carta comienza con una línea para el valor (2, 3, ..., 14) seguida de una línea para el palo (picas: P, diamantes: D, tréboles: T y corazones: C).

### Salida

La salida debe contener M líneas, una por cada mano, con alguno de los cuatro mensajes según sea el caso:
- Escalera normal
- Color
- Escalera de color
- Escalera real
- Otra mano

|Ejemplo de entrada|Ejemplo de salida|
|------------------|-----------------|
|2[7D,6C,9T,8P,5T,11C,10C,13C,14C,12C]|[Escalera normal,Escalera real]|


```python

```
