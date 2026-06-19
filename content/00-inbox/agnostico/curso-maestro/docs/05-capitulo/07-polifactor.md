# Polifactor

Se dice que un entero A es polifactor de los valores enteros entre $P y Q (1 ≤ P < Q ≤ A)$ si $A$ es divisible por todos los valores enteros entre $P y Q$ (incluyéndolos) pero dejando cada vez el factor correspondiente.

- Así por ejemplo, 24 es Polifactor entre 2 y 4 porque:
  - 24 es divisible por 2 y
  - el factor restante 12 $\frac{24}{2}$ es divisible por 3 y
  - el factor restante 4 $\frac{12}{3}$ es divisible por 4

- De igual manera, 720 es Polifactor entre 3 y 6 porque:
  - 720 es divisible por 3 y
  - el factor restante 240 (720/3) es divisible por 4 y
  - el factor restante 60 (240/4) es divisible por 5 y
  - el factor restante 12 (60/5) es divisible por 6
- Mientras que 60 no es Polifactor entre 2 y 5 porque:
  - 60 es divisible por 2 y
  - el factor restante 30 (60/2) es divisible por 3 pero
  - el factor restante 10 (30/3) NO es divisible por 4 (sin importar que si lo sea por 5)

Haz un programa para que, dados una serie de valores de A, P y Q, mostrar si se tratan o no de Polifactores.

#### Entrada
La entrada comienza con una línea que contiene la cantidad C de casos (no más de 100). Por cada caso siguen tres líneas, cada una con un valor entero positivo. La primera con A, la segunda con P y la tercera con Q.

#### Salida
La salida debe contener C líneas, una por cada caso con el mensaje (sin comillas): 'A es polifactor entre P y Q' o 'A no es polifactor entre P y Q'.

**Ejemplo de entrada:**
[2,60,2,5,3628800,1,10]

**Ejemplo de salida:**
```
60 no es polifactor entre 2 y 5
3628800 es polifactor entre 1 y 10
```