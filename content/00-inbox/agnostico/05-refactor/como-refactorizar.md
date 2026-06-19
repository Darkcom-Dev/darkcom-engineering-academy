---
title: Cómo refactorizar
tags:
  - poo
  - refactorizacion
course: curso-python/05-refactor
---

La refactorización debe hacerse como una serie de pequeños cambios, 
cada uno de los cuales mejora ligeramente el código existente mientras 
sigue dejando el programa en funcionamiento.

## Lista de verificación de una refactorización realizada correctamente
### El código debe volverse más limpio.
Si el código sigue siendo tan poco claro después de la refactorización... 
bueno, lo siento, pero acabas de desperdiciar una hora de tu vida. 
Trata de averiguar por qué sucedió esto.

Esto ocurre con frecuencia cuando te alejas de la refactorización con pequeños 
cambios y mezclas un montón de refactorizaciones en un solo gran cambio. 
Así que es muy fácil perder la cabeza, especialmente si tienes un límite de tiempo.

Pero también puede ocurrir al trabajar con un código extremadamente desordenado. 
Sea lo que sea que mejores, el código en su conjunto sigue siendo un desastre.

En este caso, vale la pena pensar en reescribir por completo partes del código. 
Pero antes de eso, debes haber escrito pruebas y reservado un buen tiempo. 
De lo contrario, terminarás con los resultados de los que hablamos en el primer párrafo.

### No se debe crear nueva funcionalidad durante la refactorización.
No mezcles la refactorización y el desarrollo directo de nuevas funciones. 
Trata de separar estos procesos al menos dentro de los límites de confirmaciones individuales.

### Todos los tests existentes deben pasar después de la refactorización.
Hay dos casos en los que las pruebas pueden fallar después de la refactorización:

- _Cometiste un error durante la refactorización._ Este es obvio: 
sigue adelante y arregla el error.

- _Tus pruebas eran demasiado detalladas._ Por ejemplo, 
estabas probando métodos privados de clases.

En este caso, las pruebas son las culpables. 
Puedes refactorizar las pruebas en sí mismas o escribir un conjunto completamente 
nuevo de pruebas de nivel superior. 
Una gran manera de evitar este tipo de situación es escribir pruebas al _estilo BDD_.
