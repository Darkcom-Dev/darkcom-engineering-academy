# Un elefante se balanceaba

"Un elefante se balanceaba"
"sobre la tela de una araña,"
"como veía que resistía"
"fue a llamar a otro elefante."
"Dos elefantes se balanceaban,"
"sobre la tela de una araña,"
"como veían que resistía"
"fueron a llamar a otro elefante."
"Tres elefantes se balanceaban ..."

Todos conocemos la canción infantil pero el interrogante, ya siendo un poco más adultos, es: si cada elefante tiene un peso individual y la telaraña tiene una capacidad de carga limitada ¿cuál será la cantidad máxima de elefantes que soporta antes de romperse?

#### Entrada

La entrada comienza con una línea que contiene un valor positivo que corresponde a la capacidad máxima de carga de la telaraña en kgs. Luego sigue una línea que contiene un valor entero positivo N que corresponde a la cantidad de elefantes sobre los que se tiene datos. Luego siguen N líneas, cada una con un valor positivo correspondiente al peso en kgs de cada elefante.

#### Salida

La salida contiene una única línea con la cantidad máxima de elefantes que soporta la telaraña sin romperse considerando que los elefantes deben subirse uno a uno y en el orden que aparece en la entrada. Los datos de entrada garantizan que eventualmente la telaraña se rompe por lo que esa cantidad siempre será inferior a N.

| Ejemplos de entrada                                | Ejemplos de salida |
| -------------------------------------------------- | ------------------ |
| 1000.0 - 5 - 100.0 - 200.0 - 300.0 - 400.0 - 500.0 | 4                  |
| 500.0 - 3 - 450.0 - 350.0 - 250.0                  | 1                  |
