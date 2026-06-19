# Lección 03: Parámetros y Argumentos

Los parámetros son "huecos" o etiquetas que definimos en una función para que pueda recibir información externa y trabajar con ella.

## Definición vs Invocación
- **Parámetros:** Son los nombres que usamos al *definir* la función.
- **Argumentos:** Son los valores reales que pasamos al *invocar* la función.

### Ejemplo de Código
```javascript
// numero1 y numero2 son PARÁMETROS
function sumar(numero1, numero2) {
    // Usamos el símbolo + al inicio para convertir el texto en número
    return +numero1 + +numero2;
}

// 10 y 5 son ARGUMENTOS
let resultado = sumar(10, 5); 
```

### El Truco de la Conversión (`+`)
Cuando leemos valores de un `input`, JavaScript los trata como **texto** (Strings). Si intentas sumar `"5" + "5"`, el resultado será `"55"`.
Para solucionarlo, anteponemos un signo `+` al valor para convertirlo en un **Número**.

```javascript
let numeroReal = +elementoInput.value;
```

---
[[04-02-return|<- Anterior]] | [[00-indice-curso|Índice]] | [[04-04-calcular-combustible|Siguiente: Práctica de Parámetros ->]]
