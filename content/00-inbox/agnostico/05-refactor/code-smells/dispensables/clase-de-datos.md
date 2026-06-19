---
title: Clase de datos
tags:
  - poo
  - refactorizacion
  - dispensables
course: curso-python/05-refactor
---
# Clase de datos
## Signos y síntomas

Una clase de datos se refiere a una clase que contiene solo campos y métodos crudos para acceder a ellos (getters y setters). Estos son simplemente contenedores de datos utilizados por otras clases. Estas clases no contienen ninguna funcionalidad adicional y no pueden operar de manera independiente sobre los datos que poseen.

## Razones del problema

Es normal que una clase recién creada contenga solo unos pocos campos públicos (y tal vez incluso un puñado de getters/setters). Pero el verdadero poder de los objetos es que pueden contener tipos de comportamiento u operaciones en sus datos.

## Tratamiento

Si una clase contiene campos públicos, **use Encapsulate Field** para ocultarlos del acceso directo y requiera que el acceso se realice solo a través de getters y setters.

Use **Encapsulate Collection** para los datos almacenados en colecciones (como matrices).

Revise el código del cliente que utiliza la clase. En él, puede encontrar funcionalidad que estaría mejor ubicada en la propia clase de datos. Si este es el caso, use **Move Method** y **Extract Method** para migrar esta funcionalidad a la clase de datos.

Después de que la clase se haya llenado con métodos bien pensados, es posible que desee deshacerse de los viejos métodos de acceso a datos que brindan acceso excesivamente amplio a los datos de la clase. Para esto, pueden ser útiles Remove Setting Method y Hide Method.

## Beneficios

Mejora la comprensión y organización del código. Las operaciones sobre datos específicos ahora se agrupan en un solo lugar, en lugar de manera desordenada en todo el código.

Ayuda a detectar la duplicación de código del cliente.
