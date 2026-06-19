# Código Morse

En 1837, Samuel Morse y Alfred Vail estaban trabajando en un sistema de telégrafo eléctrico y decidieron usar un método por el cual cada símbolo era transmitido de forma individual como una combinación de rayas y puntos, es decir, señales telegráficas que se diferencian en el tiempo de duración de la señal activa (punto = señal corta, raya = señal larga). En una primera versión solo se codificaban números, pero luego en 1841 se expandió para incluir letras y otros signos de puntuación, creando así el código actual que se conoce como American Morse Code.

Considerando solamente los códigos correspondientes a las siguientes letras y signos de puntuación, ¿harías un programa para traducir mensajes?

|c|code |c|code |c|code |c|code |c|code |
|-|-----|-|-----|-|-----|-|-----|-|-----|
|A|.-   |G|--.  |M|--   |S|...  |Y|-.--  |
|B|-... |H|.... |N|-.   |T|-    |Z|--..  |
|C|-.-. |I|..   |O|---  |U|..-  |.|.-.-.-|
|D|-..  |J|.--- |P|.--. |V|...- |,|-.-.--|
|E|.    |K|-.-  |Q|--.- |W|.--  ||
|F|..-. |L|.-.. |R|.-.  |X|-..- ||

Debes tener en cuenta dos consideraciones:
1) Independiente que en el mensaje aparezcan, no se hace diferenciación entre minúsculas y mayúsculas
2) Entre cada carácter codificado debe aparecer un espacio en blanco para saber dónde termina uno y empieza el otro. Al final del mensaje codificado también debe haber un espacio en blanco
3) Los espacios en blanco del mensaje original se codificarán con el carácter /

### Entrada

La entrada comienza con una línea que contiene la cantidad N de mensajes a codificar, no más de 1000. Luego siguen N Líneas, cada una con el mensaje correspondiente.

Todos los mensajes contendrán únicamente los caracteres mostrados en la tabla.

### Salida

Por cada caso de prueba la salida debe contener una línea con el mensaje codificado correspondiente. Entre caso y caso se dejará una línea en blanco y habrá también una línea en blanco después del último mensaje.

**Ejemplo de entrada**

3
- Hola mundo
- La mariposa revolotea, como si desesperara en este mundo.
- Este es un mensaje programado en Python, pero codificado en morse.

**Ejemplo de salida**
```
.... --- .-.. .- / -- ..- -. -.. ---

.-.. .- / -- .- .-. .. .--. --- ... .- / .-. . ...- --- .-.. ---
- . .- -.-.-- / -.-. --- -- --- / ... .. / -.. . ... . ... .--. .
.-. .- .-. .- / . -. / . ... - . / -- ..- -. -.. --- .-.-.-
. ... - . / . ... / ..- -. / -- . -. ... .- .--- . / .--. .-. ---

--. .-. .- -- .- -.. --- / . -. / .--. -.-- - .... --- -. -.-.--
/ .--. . .-. --- / -.-. --- -.. .. ..-. .. -.-. .- -.. --- / . -.
/ -- --- .-. ... . .-.-.-
```

```python

```
