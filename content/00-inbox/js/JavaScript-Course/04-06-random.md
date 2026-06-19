# Lección 06: Números Aleatorios (`Math.random`)

Generar números al azar es vital para juegos, simulaciones y selección de datos aleatorios.

## ¿Cómo funciona `Math.random()`?
Este método devuelve un número decimal entre **0** (incluido) y **1** (excluido).

### Generar un rango (Fórmula Maestra)
Para obtener un número entero entre un valor `mínimo` y un `máximo`, usamos esta lógica:

```javascript
function crearAleatorio(minimo, maximo) {
    // Sumamos 1 al máximo para que sea inclusive
    let rango = (maximo + 1) - minimo;
    let aleatorio = Math.floor(Math.random() * rango) + minimo;
    return aleatorio;
}

console.log(crearAleatorio(1, 10)); // Devuelve un número del 1 al 10
```

### Desglose de la Fórmula
1. `Math.random()`: Decimal entre 0 y 0.99.
2. `* rango`: Escala el número al tamaño de nuestro intervalo.
3. `+ minimo`: Desplaza el inicio del intervalo al número deseado.
4. `Math.floor()`: Quita los decimales para obtener un entero.

---
[[04-05-math|<- Anterior]] | [[00-indice-curso|Índice]] | [[04-07-calculadora|Siguiente: Proyecto Calculadora ->]]
