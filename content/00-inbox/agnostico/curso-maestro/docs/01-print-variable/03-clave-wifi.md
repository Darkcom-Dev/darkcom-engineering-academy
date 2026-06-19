<h1>Clave Wifi</h1>
        <p>
            Aburrida de que siempre le preguntaran la clave del WiFi, cierta profesora de matemáticas
            puso el siguiente cartel:
        </p>
> Passsword:
>$$ \frac{\int x(x^2 + 5)^2 dx - 3 \int x(x^2 + 5)^{-1 2} dx}{\int\frac{x[(x^2 + 5)-3]}{\sqrt{x^2 + 5}}dx}$$

Supongamos que queremos hacer algo similar, pero en vez de una ecuación integral como la de la profesora, vamos a usar una operación aritmética.

La idea es que aproveches lo que aprendimos sobre precedencia de operadores y que te inventes una operación para “esconder” la clave 123.

Pero atención, **sé lo más creativo que puedas**. Mi solución por ejemplo es:

$$\frac{17}{3+12^2}-(2(5+8))$$

Debes hacer entonces un programa para imprimir el resultado de la operación que te
inventes. En mi caso por ejemplo, dicho programa sería:<br>
`print(17//3+12**2-((5+8)*2))`<br>
Recuerda, DEBES inventar tu propia operación y ten presente que el valor que se imprima
debe ser entero (no 123.0).

```python
def clave_wifi():
    # Escribe tu código aqui
    pass
```

```python
clave_wifi()
```


    123.0


```python
# Ejecuta esta celda apenas termines la funcion.

import base64
exec(base64.b64decode('YXNzZXJ0IGNsYXZlX3dpZmkoKSA9PSAxMjMuMCwgJ0xhIGNsYXZlIFdpRmkgY2FsY3VsYWRhIG5vIGVzIGNvcnJlY3RhJztwcmludCgnRmVsaWNpZGFkZXMsIGhhcyB0ZXJtaW5hZG8gZWwgZWplcmNpY2lvIScp',))
```


```python
assert clave_wifi() == 123.0, 'La clave WiFi calculada no es correcta';
print('Felicidades, has terminado el ejercicio!')
```

    Felicidades, has terminado el ejercicio!

