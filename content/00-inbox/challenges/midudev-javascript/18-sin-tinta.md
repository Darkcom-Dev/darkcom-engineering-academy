---
tipo: reto
dificultad: 3
---

# 🖨️ Día 18 — Sin Tinta

---

## 🎯 Objetivo

Dado un dígito que falta de tinta y una cantidad de números a imprimir, devolver todos los números en ese rango que contengan dicho dígito.

---

## 📚 Conocimientos Previos

- Creación de arrays usando `Array.from()` y métodos similares
- Uso de arrow functions y callbacks
- Conversión entre números y strings (`toString()`)
- Métodos de búsqueda en strings (`includes()`)
- Método `filter()` para arrays
- Manipulación básica de rangos numéricos

---

## 📖 Contexto

En la fábrica se quedan sin tinta de un número. Recibe el dígito que falta (`dry`) y la cantidad de códigos a imprimir (`numbers`). Devuelve los números que contienen ese dígito.

Ejemplos:
```javascript
dryNumber(1, 15) // [1, 10, 11, 12, 13, 14, 15]
dryNumber(2, 20) // [2, 12, 20]
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `dryNumber(dry, numbers)` que:
1. Reciba un dígito `dry` (número del 0-9) que representa el que falta de tinta
2. Reciba un número `numbers` que representa la cantidad de códigos a imprimir (del 1 a `numbers`)
3. Devuelva un array con todos los números en el rango [1, numbers] que contengan el dígito `dry` en su representación decimal

### Función sugerida

```javascript
function dryNumber(dry, numbers) {
  return Array.from({ length: numbers }, (_, i) => i + 1)
    .filter(n => n.toString().includes(dry.toString()))
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Técnicas de generación de secuencias numéricas: `Array.from()`, bucles tradicionales, `Array.prototype.fill()`
- Patrones de filtrado y selección: uso de `filter()`, condiciones complejas, funciones predicado
- Conversión entre tipos: número a string y viceversa, manejo de bases numéricas
- Búsqueda de subcadenas: algoritmos de búsqueda eficientes, expresiones regulares vs. métodos nativos
- Optimización de operaciones en arrays: encadenamiento de métodos, evaluación perezosa
- Análisis de complejidad: tiempo y espacio en relación al tamaño de la entrada
- Manejo de casos edge: dígitos fuera de rango, valores cero o negativos

---

## 🔗 Retos similares
