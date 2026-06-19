# Viajando por el multiverso

Si no has visto la serie de animación para adultos “Rick and Morty” te bastará saber para este ejercicio que Rick inventó un dispositivo que abre agujeros de gusano con los que puede desplazarse a través del espacio y entre dimensiones. A cada agujero de gusano se le llama “portal” y a cada desplazamiento por un portal se le llama “salto”. De hecho, parte importante de la trama es que existen infinitos universos paralelos. Así, el universo que conocemos es uno de tantos por lo que Rick a denominado a nuestro planeta como Planeta “Tierra del Universo C-137”.


<img src='https://www.radionica.rocks/series/rick-morty-pelicula'>
Fuente: 


Casi siempre el dispositivo funciona bien, pero a veces presenta desperfectos y abre por sí mismo portales de modo que a los protagonistas puede dificultárseles volver a su universo. Dado un conjunto de portales abiertos ¿podrías determinar si, dando saltos por esos portales siendo el primero el que tiene como origen C-137, los protagonistas podrían volver o no y en cuantos saltos?

### Entrada

La entrada comienza con una línea que contiene la cantidad C de casos. Cada caso comienza con una línea que contiene la cantidad N de portales abiertos (por lo menos 2) seguida por N líneas con cada salto, los cuales se componen por dos datos separados entre sí por un espacio en blanco: el universo de origen y el de destino. Por simplicidad consideremos que el origen del primer salto siempre es el universo C-137, que el de destino nunca es igual al de origen y que solo se pasa una vez por cada universo. Todo universo se identifica de manera única por una serie de caracteres en mayúsculas seguidos de un guion seguido de un numero entero positivo.

### Salida

La salida debe contener una C líneas, una por cada caso y con el mensaje (sin comillas):
‘Pueden volver a C-137 en X saltos’ (siendo X la cantidad correspondiente) o ‘Deambulan por el multiverso’ según sea el caso.

**Ejemplo de entrada**
```
4
3
C-137 X-5
FP-41 C-137
X-5 FP-41
2
C-137 XYZ-999
XYZ-999 ABC-123
5
C-137 A-1
A-1 B-2
D-4 C-137
E-5 F-6
B-2 D-4
4
C-137 CPP-9
PY-38 JAVA-5
CPP-9 PY-38
RUBY-2 C-137
```
**Ejemplo de salida**
```
Pueden volver a C-137 en 3 saltos
Deambulan por el multiverso
Pueden volver a C-137 en 4 saltos
Deambulan por el multiverso
```

```python

```
