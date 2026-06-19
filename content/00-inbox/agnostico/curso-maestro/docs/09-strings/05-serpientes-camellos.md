# Serpientes y camellos

Cuando aprendimos a nombrar variables en nuestros programas vimos que debemos seguir las siguientes reglas:

1. Debe iniciar con una letra como estatura, i, o x
2. Puede contener números y el carácter _ como valor_1 o 2do_valor
3. No se pueden usar nombres reservados del lenguaje como print o input
4. Son sensible a mayúscula y minúscula, por ejemplo a ≠ A, estatura ≠ Estatura

Y adicionalmente vimos que una recomendación es usar nombres mnemotécnicos.

Sin embargo, dependiendo de la complejidad de los programas, a veces usar esos nombres mnemotécnicos no es tan sencillo, sobre todo cuando contienen más de una palabra. Para estos casos, y sabiendo que no se puede usar espacios en blanco, dos de los métodos que usan más los programadores expertos son:

Snake case (método serpiente): Todas las palabras se ponen en minúscula y luego se unen entre sí con un guion bajo

Camel case (método camello): Todas las palabras se ponen en minúscula excepto la primera letra y luego se unen

Así por ejemplo, para usar una variable referida a la “sumatoria de divisores pares”, en método serpiente sería sumatoria_de_divisores_pares y en método camello SumatoriaDeDivisoresPares.

Debes hacer un programa para que, dada una variable nombrada usando el método serpiente, se traduzca a camello.

### Entrada

La entrada comienza con una línea que contiene la cantidad N de nombres de variable (no más de 100). Luego siguen N líneas con dichos nombres.

### Salida

La salida debe contener N líneas, cada una con la traducción correspondiente.

|Ejemplo de entrada         |Ejemplo de salida          |
|---------------------------|---------------------------|  
|2                          |                           |
|cantidad_de_usuarios       |CantidadDeUsuarios         |
|porcentaje_de_ganancia_neta|PorcentajeDeGananciaNeta   |

```python

```
