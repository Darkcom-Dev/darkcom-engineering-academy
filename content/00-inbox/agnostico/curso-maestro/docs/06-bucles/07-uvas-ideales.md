# Racimos de uvas ideales

Han visto los cuentos infantiles, series animadas, e incluso algunos libros de texto donde al mostrar imágenes de racimos de uvas estos siempre son “perfectos”. Viéndolos de abajo hacia arriba comienzan con una única uva, luego hay un segundo nivel con dos uvas perfectamente centradas, luego un tercer nivel con tres y así sucesivamente.

Pensándolo de otra manera, un racimo de esos ideales tendrá una sola uva si es de un nivel, tendrá tres si es de dos niveles, tendrá 6 si es de tres y así sucesivamente.

Podrías hacer un programa para determinar, si la cantidad de uvas de un racimo es N, si este podría o no ser ideal. Así por ejemplo, si N es 10 se podría tener un racimo ideal de 4 niveles, mientras que si N fuera 20 no podría serlo pues tendría 6 niveles pero a ese sexto nivel le faltaría una uva para ser ideal.

### Entrada

La entrada contiene una serie de líneas (más de 1 y no más de 500), con el valor correspondiente de N (1 ≤ N ≤ 10000) y termina con un valor de 0 que no corresponde a un N.

### Salida

Por cada valor de N debe mostrarse una línea con el mensaje (sin comillas) 'Puede ser
un racimo ideal' o 'No puede ser un racimo ideal' según sea el caso.

|Ejemplo de entrada |Ejemplo de salida|
|-------------------|-----------------|
|15  |Puede ser un racimo ideal|
|100 |No puede ser un racimo ideal|
|1035|Puede ser un racimo ideal|
|0   ||
