# Cálculo de pago

En una determinada empresa el pago de la nómina se hace de manera semanal y su cálculo es bastante simple.

Se tiene una tarifa horaria la cual se paga al 100% a cada trabajador que labore 45 horas o menos, mientras que se paga al 150% por las horas extra, es decir, de la hora 46 en adelante.

Así por ejemplo, si la tarifa es de $10000/hora y un trabajador labora 40 horas, su pago será de $400000. Entre tanto, considerando esa misma tarifa pero 50 horas laboradas, el pago será de $525000 ($450000 por las primeras 45 horas más $75000 por las 5 horas extra)

Escribe un programa para, dada una tarifa por hora y una cantidad de horas trabajadas muestre el pago correspondiente.

#### Entrada:

La entrada contiene dos líneas, cada una con un valor entero positivo, la primera para la tarifa (siempre es un múltiplo de 100) y la segunda para la cantidad de horas.

#### Salida:
Una única línea con el valor del pago correspondiente (sin decimales) antecedido por el símbolo $ y un espacio en blanco.

| Ej. de entrada | Ej. de salida |
| -------------- | ------------- |
| 20000 - 30     | 600000        |
| 12000 - 60     | 810000        |


```python
def calculo_pago(tarifa, horas):
    if horas <= 45:
        return tarifa * horas
    else:
        return tarifa * 45 + (horas - 45) * tarifa * 1.5
```

```python
calculo_pago(12000, 60)
```


    810000.0