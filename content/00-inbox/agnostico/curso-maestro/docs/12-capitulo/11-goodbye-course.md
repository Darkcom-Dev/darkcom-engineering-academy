# Goodbye course!

Si el primer programa que hicimos en el curso fue el famoso “Hola mundo”, es apenas lógico que el último sea una despedida. Sin embargo, un mensaje como “Adiós mundo!” suena inapropiado, así que será mejor que usemos “Hasta luego curso!”.

Pero a estas alturas estarás pensando que con todo lo que has aprendido es algo demasiado fácil para ti, entonces compliquémoslo un poco, solo por diversión.

Consideremos el siguiente mensaje en cinco idiomas diferentes.

```
Hasta luego curso! Turing, puedes estar orgulloso de mi 
Goodbye course! Turing, you can be proud of me
Au revoir cours! Turing, tu peux etre fier de moi
Adeus, curso! Turing, pode se orgulhar de mim
Auf Wiedersehen! Turing, du kannst stolz auf mich sein
```

Supongamos que a cada idioma le corresponde el siguiente código: 0 para Español, 1 para Inglés, 2 para Francés, 3 para Portugués, y 4 para Alemán.

Dado un número entero, debes hacer un programa para imprimir el mensaje según el código que corresponda al residuo de la división entera de ese número por 5. El proceso se repite luego con el cociente de la división entera por 5, y así sucesivamente hasta que se llegue a un valor inferior a 5.

Así por ejemplo, si el número inicial es 1027 tendríamos:

- 1027 % 5 = 2, se imprime el mensaje en Francés
- 205 % 5 = 0, se imprime el mensaje en Español
- 41 % 5 = 1, se imprime el mensaje en Inglés
- 8 % 5 = 3, se imprime el mensaje en Portugués

Y como el cociente en este último caso es 1, no se imprime nada más

### Entrada

La entrada contiene una única línea con el valor del número inicial, el cual es mayor a cuatro e inferior a mil millones.

### Salida

La salida tendrá la secuencia de mensajes según el procedimiento descrito previamente, de a uno por línea.

**Ejemplo de entrada**

55555

**Ejemplo de salida**

```
Hasta luego curso! Turing, puedes estar orgulloso de mi
Goodbye course! Turing, you can be proud of me
Au revoir cours! Turing, tu peux etre fier de moi
Auf Wiedersehen! Turing, du kannst stolz auf mich sein
Adeus, curso! Turing, pode se orgulhar de mim
Au revoir cours! Turing, tu peux etre fier de moi
```

```python

```
