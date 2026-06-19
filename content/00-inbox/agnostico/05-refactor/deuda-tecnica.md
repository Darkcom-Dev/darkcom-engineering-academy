---
title: Deuda Técnica
tags:
  - poo
  - refactorizacion
course: curso-python/05-refactor
---

Todos hacen lo posible para escribir un codigo excelente desde cero. 
Probablemente no haya ningun programador que escriba intencionalmente un 
codigo poco limpio en detrimento del proyecto. Pero, 
en que momento el codigo limpio se convierte en codigo poco limpio?

La metafora de la "deuda tecnica" en relacion al codigo poco limpio fue propuesta 
originalmente por Ward Cunningham.

Si tomas un prestamo de un banco, esto te permite hacer compras mas rapido. 
Pagas extra para acelerar el proceso, no solo pagas el capital, 
sino tambien el interes adicional del prestamo. 

Es innecesario decir que incluso puedes acumular tanto interes que la cantidad 
de interes supera tus ingresos totales, lo que hace imposible la devolucion total.

Lo mismo puede ocurrir con el codigo. 
Puedes acelerar temporalmente sin escribir pruebas para nuevas funciones, 
pero esto ralentizara tu progreso cada dia hasta que finalmente pagues 
la deuda escribiendo pruebas.

## Causas de la deuda tecnica

### Presion empresarial
A veces, las circunstancias empresariales pueden obligarte a lanzar funciones antes 
de que esten completamente terminadas. En este caso, apareceran parches y soluciones 
temporales en el codigo para ocultar las partes inacabadas del proyecto.

### Falta de comprension de las consecuencias de la deuda tecnica
A veces, tu empleador puede no entender que la deuda tecnica tiene "interes" en 
la medida en que ralentiza el ritmo de desarrollo a medida que se acumula la deuda. 
Esto puede hacer que sea demasiado dificil dedicar tiempo del equipo a 
la refactorizacion porque la gestion no ve su valor.

### Falta de combate a la coherencia estricta de los componentes
Esto es cuando el proyecto se asemeja a un monolito en lugar de 
ser el producto de modulos individuales. En este caso, 
cualquier cambio en una parte del proyecto afectara a otras. 
El desarrollo en equipo se vuelve mas dificil porque es dificil aislar el 
trabajo de los miembros individuales.

### Falta de pruebas
La falta de retroalimentacion inmediata fomenta soluciones rapidas pero 
arriesgadas o soluciones temporales. En los peores casos, 
estos cambios se implementan y despliegan directamente en produccion sin pruebas previas. 
Las consecuencias pueden ser catastroficas. Por ejemplo, 
un hotfix inocente podria enviar un correo electronico de prueba extraño 
a miles de clientes o, incluso peor, vaciar o corromper una base de datos completa.

### Falta de documentacion
Esto ralentiza la introduccion de nuevas personas al proyecto y puede detener 
el desarrollo si personas clave abandonan el proyecto.

### Falta de interaccion entre los miembros del equipo
Si la base de conocimientos no esta distribuida en toda la empresa, 
las personas terminaran trabajando con una comprension desactualizada de los procesos 
y la informacion sobre el proyecto. 
Esta situacion puede empeorar cuando los desarrolladores junior son entrenados 
incorrectamente por sus mentores.

### Desarrollo simultaneo a largo plazo en varias ramas
Esto puede llevar a la acumulacion de deuda tecnica, que luego se aumenta 
cuando se fusionan los cambios. 
Cuantos mas cambios se realicen en aislamiento, mayor sera la deuda tecnica total.

### Refactorizacion tardia
Los requisitos del proyecto cambian constantemente y en algun momento 
puede ser evidente que algunas partes del codigo estan obsoletas, 
se han vuelto engorrosas y deben ser rediseñadas para cumplir con los nuevos requisitos.

Por otro lado, los programadores del proyecto estan escribiendo nuevo 
codigo todos los dias que funciona con las partes obsoletas. 
Por lo tanto, cuanto mas se retrase la refactorizacion, mas dependiente 
sera el codigo que tendra que ser reescrito en el futuro.

### Falta de monitoreo de cumplimiento
Esto sucede cuando todos los que trabajan en el proyecto escriben codigo como 
mejor les parece (es decir, de la misma forma que escribieron el ultimo proyecto).

### Incompetencia
Esto sucede cuando el desarrollador simplemente no sabe como escribir codigo decente.
