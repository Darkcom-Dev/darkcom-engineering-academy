# Relatividad especial

La teoría de la relatividad especial, también llamada teoría de la relatividad restringida, fue propuesta por Albert Einstein en 1905 y se denomina "especial" ya que solo se aplica en el caso particular donde la curvatura del espacio-tiempo producida por acción de la gravedad es irrelevante (con el fin de incluir la gravedad, Einstein formuló la teoría de relatividad general 10 años más tardes).

En resumen, esta teoría describe cómo el tiempo y el espacio no son conceptos absolutos, sino que son relativos dependiendo de la velocidad del observador. De hecho, una de sus ecuaciones más famosas es la siguiente:
$$t ′ = tγ$$

Donde 

$$γ = \frac{1}{\sqrt{1-\frac{v^2}{c^2}}}$$

se conoce como el factor de Lorentz

Sabiendo que c es la velocidad de la luz en el vacío (299792458 metros/segundo), nótese que a menor velocidad del observador v, el tiempo relativo t’ es prácticamente igual al tiempo “normal” t. De hecho, si esa velocidad es cero es fácil ver que t’ = t (que es lo mismo a decir que el factor de Lorentz en este caso es 1).

Entre tanto, si el observador pudiera por ejemplo desplazarse al 99% de la velocidad de la luz, el tiempo relativo sería casi 7 veces más lento ( γ = 7.089 ).

Para que te vayas familiarizando con esta teoría, debes hacer un programa (preferiblemente incluyendo la definición de una función) para calcular el factor de Lorentz para diferentes valores de velocidad v (0 ≤ v < c). Para facilitar la interpretación, esa velocidad estará expresada en kilómetros/hora que es una medida mucho más familiar para nosotros que metros/segundo.

Así por ejemplo, sabríamos que si nos desplazamos a 50 km/h, el tiempo se siente 1.000000000000001 veces más lento ¿lo has notado?

### Entrada

La entrada comienza con una línea que contiene la cantidad N de valores de velocidad a evaluar (no más de 1000). Luego siguen N líneas con dichos valores. Recuerda que esas velocidades están expresados en km/h por lo que debes hacer la conversión a m/s para calcular el factor.

### Salida

La salida debe contener N líneas con el correspondiente factor de Lorentz redondeado a 15 cifras decimales.

|Ejemplo de entrada|Ejemplo de salida|
|-|-|
|3          |1.000000000000006|
|120.0      |1.000000000005563|
|3600.0     |2.658655750196292|
|999999999.0|


```python

```
