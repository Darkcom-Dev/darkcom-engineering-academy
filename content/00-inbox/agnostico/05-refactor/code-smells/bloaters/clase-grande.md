---
title: Clase grande
tags:
  - poo
  - refactorizacion
  - bloaters
course: curso-python/05-refactor
---
# Clase grande
## Señales y síntomas

Una clase contiene muchos campos/métodos/líneas de código.

## Razones del problema

Las clases generalmente comienzan siendo pequeñas. Pero con el tiempo, se vuelven infladas a medida que el programa crece.

Como ocurre con los métodos largos, los programadores generalmente encuentran menos agotador mentalmente colocar una nueva función en una clase existente que crear una nueva clase para la función.

## Tratamiento

Cuando una clase tiene demasiados roles funcionales, piensa en dividirla:

Extraer clase ayuda si parte del comportamiento de la clase grande puede separarse en un componente separado.

Extraer subclase ayuda si parte del comportamiento de la clase grande se puede implementar de diferentes maneras o se utiliza en casos raros.

Extraer interfaz ayuda si es necesario tener una lista de las operaciones y comportamientos que el cliente puede usar.

Si una clase grande es responsable de la interfaz gráfica, se puede intentar mover algunos de sus datos y comportamiento a un objeto de dominio separado. Al hacerlo, puede ser necesario almacenar copias de algunos datos en dos lugares y mantener los datos consistentes. Datos observados duplicados ofrece una forma de hacer esto.

## Beneficio

La refactorización de estas clases evita que los desarrolladores necesiten recordar un gran número de atributos para una clase.

En muchos casos, dividir clases grandes en partes evita la duplicación de código y funcionalidad.
