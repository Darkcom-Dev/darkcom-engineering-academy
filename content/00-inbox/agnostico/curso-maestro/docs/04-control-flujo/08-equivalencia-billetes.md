# Equivalencia en billetes.

Supongamos que nos contratan para diseñar el software de un cajero automático. Lo primero que tenemos que hacer es que, dada una cantidad de dinero que quiere retirar el usuario, la cual siempre será múltiplo de mil y no mayor a un millón, se debe determinar la mínima cantidad de billetes en la que se le entregará esa cantidad. Para esto debemos tener en cuenta que las denominaciones disponibles de los billetes son $1000, $2000, $5000, $10000, $20000, y $50000.

Así por ejemplo, si el cliente va a retirar $188000, la mínima cantidad de billetes será:

3 de $50000 
1 de $20000 
1 de $10000 
1 de $5000 
1 de $2000 
1 de $1000 

#### Entrada:
La entrada contiene una única línea con un valor entero que corresponde a la cantidad a retirar.
#### Salidas:
La equivalencia en billetes correspondiente, de a uno por línea como se mostró anteriormente y sin mostrar valores nulos para la cantidad de billetes de una determinada denominación.


| Ej. de entrada | Ej. de salida                               |
| -------------- | ------------------------------------------- |
| 1000000        | 20 de $50000                                |
| 12000          | 1 de $10000  <br>1 de $2000                 |
| 26000          | 1 de $20000  <br>1 de $5000  <br>1 de $1000 |


```python
def equivalencia_billetes(cantidad):
    billetes = [50000, 20000, 10000, 5000, 2000, 1000]
    billetes_cant = [0, 0, 0, 0, 0, 0]
    for i in range(len(billetes)):
        billetes_cant[i] = cantidad // billetes[i]
        cantidad = cantidad % billetes[i]
    return billetes_cant
```

```python
equivalencia_billetes(188000)
```


    [3, 1, 1, 1, 1, 1]


