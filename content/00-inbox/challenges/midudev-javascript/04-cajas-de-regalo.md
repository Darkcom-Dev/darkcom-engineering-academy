---
tipo: reto
dificultad: 3
---

# 📦 Día 4 — Cajas dentro de Cajas

---

## 🎯 Objetivo

Determinar si es posible empaquetar todas las cajas en una sola, dado que una caja entra en otra solo si todos sus lados son estrictamente menores y no se permiten rotaciones.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y objetos en JavaScript
- Métodos de array como `sort()` y `every()`
- Operadores de comparación y lógica condicional
- Conceptos de ordenamiento y comparación de propiedades múltiples

---

## 📖 Contexto

Santa Claus necesita revisar sus cajas. Cada caja tiene medidas `{l, w, h}` (largo, ancho, alto). Una caja entra en otra si **todos sus lados son menores**. Las cajas **no se pueden rotar**.

Escribe una función que determine si es posible empaquetar **todas** las cajas en una sola.

Ejemplos:
```javascript
fitsInOneBox([
  { l: 1, w: 1, h: 1 },
  { l: 2, w: 2, h: 2 }
]) // true

fitsInOneBox([
  { l: 1, w: 1, h: 1 },
  { l: 2, w: 2, h: 2 },
  { l: 3, w: 1, h: 3 }
]) // false
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `fitsInOneBox(boxes)` que reciba un array de objetos con propiedades `l`, `w`, y `h` representing las dimensiones de las cajas, y devuelva un boolean indicando si es posible nesting todas las cajas en una sola.

### Función sugerida

```javascript
function fitsInOneBox(boxes) {
  const sorted = [...boxes].sort((a, b) => a.l - b.l)
  return sorted.every((box, i) => {
    if (i === 0) return true
    return box.l > sorted[i-1].l
        && box.w > sorted[i-1].w
        && box.h > sorted[i-1].h
  })
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de ordenamiento y sus aplicaciones en problemas de comparación múltiple
- Técnicas de validación de secuencias: verificar si una secuencia es estrictamente creciente en múltiples dimensiones
- Problemas de enriquecimiento de cajas (box stacking) y sus variantes
- Patrones de diseño para funciones de validación de propiedades múltiples
- Complejidad algorítmica: análisis del costo de ordenamiento vs. validación lineal

---

## 🔗 Retos similares
