# Obsesión de familia

¿Recuerdas a Carolina, la de la obsesión con el número 3? Pues al parecer es un mal de familia porque su primo Daniel también tiene una, pero con el 123.

En el caso de Daniel su obsesión llegó a tal extremo que decidió ir a la Universidad a estudiar la carrera de Matemáticas con el único fin de demostrar que la siguiente función, inventada por él, y cuyo dominio son los enteros no negativos, siempre converge:

$$F(X) ={{0 si X es múltiplo de 123}{1 + F(X + 23) en caso contrario}}$$

Nótese que los valores involucrados en esa función no están ahí por nada. El 123 aparece dos veces, primero de manera explícita y luego un tanto “escondido”.

Si se graduó o no, nunca lo sabremos. Lo importante ahora es que hagas un programa para evaluar la función de Daniel para un conjunto de valores.

### Entrada

La entrada comienza con una línea que contiene la cantidad N de valores a evaluar (no más de 1000). Luego siguen N líneas, cada una con un valor entero no negativo.

### Salida

La salida debe contener N líneas, cada una con la evaluación de la función en cada uno de los valores de entrada.

|Ejemplo de entrada|Ejemplo de salida|
|------------------|-----------------|
|4  |0   |
|123|1   |
|100|0   |
|246|117 |
|999|    |

```python

```
