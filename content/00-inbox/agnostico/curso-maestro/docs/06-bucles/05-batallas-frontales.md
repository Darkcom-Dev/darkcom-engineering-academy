# Batallas frontales

¿Han visto esas películas de guerra en la segunda mitad del siglo 19, como fue el caso de la guerra civil estadounidense, en donde el pelotón de cada uno de los bandos se para en línea recta de frente al pelotón del otro bando que lo hace de la misma manera y luego se disparan entre sí?


![image.png](00-inbox/ciencia-de-datos/02-recopilacion-de-datos/02-api/assets/image.png)

Fuente: https://commons.wikimedia.org/wiki/File:American_Civil_War_Re-enactment_Bath.jpg

Uno piensa para sus adentros ¿por qué hacen eso si es una muerte segura? ¿por qué no corren en zigzag, se tiran al suelo, o hacen cualquier cosa para quedar un poco menos expuestos? En fin, una de las muchas tonterías de las guerras.

Y parece que la estupidez no es exclusiva de los humanos. En un planeta de una galaxia lejana se lleva a cabo una guerra con batallas similares pero un poco más ridícula (si es que eso es posible). Los dos bandos envían pelotones con exactamente la misma cantidad de soldados. Ambos pelotones se colocan en línea recta uno al frente del otro, pero ordenan a sus soldados según su estatura. Luego comienzan los disparos, donde todos los soldados tienen que disparar al mismo tiempo pero no lo hacen de forma frontal sino diagonal. Más específicamente, el soldado de menor estatura de un bando se dispara con el de mayor estatura del otro, el segundo menor con el segundo mayor y así sucesivamente. Por las leyes de la probabilidad que solo aplican a ese planeta resulta que si dos soldados que se enfrentan ambos tienen una estatura par (expresada en centímetros sin decimales), o ambos tienen una estatura impar, ambos quedan exterminados, mientras que si la estatura de uno es par y la de otro es impar ambos sobreviven.

¿Harías un programa para, dadas las estaturas de los soldados de los dos bandos, determinar cuántos soldados en total sobreviven?

### Entrada

La entrada comienza con una línea que contiene el valor N de soldados de cada bando (1 ≤ N ≤ 1000). Luego siguen N líneas con valores enteros positivos que representan las estaturas de uno de los bandos, seguidas de otras N líneas con las estaturas del otro.

### Salida

La salida debe tener una única línea con el mensaje: (sin comillas) 'Sobreviven X soldados' siendo X el valor a determinar.

|Ejemplo de entrada |Ejemplo de salida|
|-------------------|-----------------|
|4  |Sobreviven 2 soldados|
|180||
|175||
|186||
|179||
|181||
|173||
|181||
|170|| 
