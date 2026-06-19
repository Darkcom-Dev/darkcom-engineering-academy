---
tipo: reto
dificultad: 5
---


# Reto Módulo 6:
# Simulador Financiero

## Objetivo

- Utilizar los conocimientos adquiridos durante las semanas uno, dos, tres y cuatro para solucionar problemas.
- Implementar programas con expresiones lógicas para resolver un problema que involucre la toma de decisiones.
- Descomponer un problema en subproblemas más pequeños y manejables para facilitar la implementación del programa.
- Construir funciones con parámetros para organizar el código fuente y facilitar la reutilización de código.
- Invocar funciones con argumentos válidos para facilitar la comprensión y el seguimiento de Programas.
- Demostrar la importancia de reutilizar de código mediante la implementación de funciones en módulos propios.
- Aplicar la estrategia de dividir y conquistar para solucionar problemas.
- Aplicar tipos de datos, funciones, ciclos y condicionales.

## Descripción del Reto

Tenemos pensado solicitar un crédito a un banco, esta como siempre es una decisión que se debe pensar dos veces. Para esto nos apoyamos en los simuladores financieros que nos ofrece nuestro banco, pero notamos que estos simuladores solo muestran una simulación si cancelamos cada cuota como esta programada, creemos que podemos hacer algo mejor para jugar con otras variables como los pagos a capital o el costo de los seguros.

Dicen que en alguna ocasión le preguntaron a Albert Einstein cuál era la fuerza más poderosa del universo, y el respondió “El interés compuesto”, esto lo tomo como un mito popular, quedará en nosotros verificar si es cierto.

Para hacer nuestro simulador necesitamos saber que es el interés compuesto y como calcularlo para poder controlarlo, este interés es el usado por las entidades financieras hoy por hoy y lo podemos entender de la siguiente manera:

- El día 0, es el día en el que se deposita el dinero a la cuenta bancaria del cliente.
- El día 1, debemos el capital que nos depositaron más el interés del día.
- El día 2, debemos el capital más el interés del día 1, más el interés del día 2, que es calculado con la suma del capital más el interés del día 1.
- Y así los días siguientes...

### ¡Momento!
Es decir que el interés que se calcula en los días siguientes después del día 1 lleva el acumulado del interés de los días anteriores. **¡Es un interés sobre el
interés!**

Dado, que nos comenzamos a interesar, buscamos nueva documentación sobre el tema para darnos una idea de cómo funciona esto:

[https://www.pnc.com/insights/es/personal-finance/save/what-is-compound-interest.html]
[https://exceltotal.com/como-calcular-el-interes-compuesto-en-excel/]

Después de entender un poco sobre el interés compuesto y como se calcula, podemos determinar que la formula que necesitamos para nuestra calculadora es:
`capital_final = capital_inicial ( 1 + interés ) ** períodos`

### Respiremos

Parece una formula siempre, y que podemos usar para crear nuestro simulador, y es cierto, solo debemos tener en cuenta valores adicionales que siempre van acompañados por los créditos, estos son los seguros, debemos solicitar el valor del seguro en nuestra calculadora, este valor será cobrado en cada cuota mensual de nuestra calculadora.

Y necesitamos agregar la posibilidad de hacer abonos adicionales a capital, este va ha ser nuestro diferenciador con las calculadoras que los bancos nos ofrecen. Esto con el fin de ver el resultado de diferentes abonos a capital.

Nos solicitan completar el código faltante, que son las funciones que se encuentran sin comentar, estas funciones ya son llamadas y la interfaz de usuario ya se encuentra programada. Sabemos que el resultado debe ser el siguiente después de completado el código:

[res/reto1_Caluculadora_financiera.png]

### Delimitemos el problema
Esta va ha ser nuestra versión beta, entonces mantengamos lo simple, usemos las siguientes restricciones:

- Calcularemos todo en meses, (podemos en días, pero será para versiones futuras).
- Usaremos el interés Efectivo Anual.
- Asumiremos que vamos a pagar una cuota por mes, de manera cumplida y sin atrasarnos, por lo que no calcularemos intereses en mora.
- El Frontend ya se encuentra desarrollado por otro programador, no debemos modificarlo.

## Aspectos a tener en cuenta

Para tener en cuenta:
- No modificar las firmas de los métodos. Esto incluye modificar nombres de funciones, agregar parámetros.
- Ya las funciones están llamadas en el flujo que establecieron como equipo de trabajo, no modifiquen el orden del flujo.
- Debemos redondear a 2 decimales, justo antes de retornar la información de los métodos.
- Debemos documentar los métodos.
- La entrada de datos debe tener, los siguientes dados:
	° El monto para solicitar (float)
	° El interés en efectivo anual. (float). En porcentaje si es 15% EA se agrega en el input el número 15. Y posterior lo dividimos por 100 para obtener el valor en decimales 0.15 para 15%.
	° El número de meses en los que queremos realizar el pago. (int)
	° Valor del seguro obligatorio, dado por el banco. Ejemplo: 20.000 pesos. Va ha ser un valor fijo a pagar en la cuota mensual.
	° Valor del abono a capital, si es vacío lo tomamos como 0.
- El sistema ya calcula el valor de la cuota mínima a pagar. (OJO: el valor esta redondeado en 2 decimales, por lo que puede traer una pequeña diferencia en la última cuota de pago, se debe validar el monto final a pagar)
- Firmar el código, en el archivo view.py (función firma) debemos colocar el nombre completo del creador del código.
- No modificar las funciones ya creadas, a menos que sea un print o algo que les ayude a verificar la solución.
- Los datos resultantes de la simulación deben ser retornados en una lista con la siguiente forma:
```
[
	{
		"mes": 1,
		"saldo_inicial": 15000000,
		"intereses": 202945.83,
		"total_cuota": 388652.77,
		"abono_capital": 5000000,
		"saldo_despues_pago": 9836293.06
	},
	{
		"mes": 2,
		"saldo_inicial": 9836293.06,
		"intereses": 133082.31,
		"total_cuota": 388652.77,
		"abono_capital": 5000000,
		"saldo_despues_pago": 4602722.6
	},
	{
		"mes": 3,
		"saldo_inicial": 4602722.6,
		"intereses": 62273.56,
		"total_cuota": 388652.77,
		"abono_capital": 4298343.39,
		"saldo_despues_pago": 0.0
	}
]

---

## 🔗 Retos financieros similares
- [[05-reto-calculadora-financiera]] — Calculadora financiera básica
- [[05-calculadora-financiera]] — MVC + Matplotlib
- [[06-2-graficador-financiero]] — Gráficas financieras
- [[04-reportes]] — Reportes CSV
- [[reto-supertiendas]] — Gestión financiera
- [[../midudev-javascript/14-el-mejor-camino]] — Optimización de rutas```

