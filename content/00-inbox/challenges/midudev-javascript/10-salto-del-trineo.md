---
tipo: reto
dificultad: 3
---

# ⛷️ Día 10 — Salto del Trineo

---

## 🎯 Objetivo

Verificar que una secuencia de alturas forme una parábola válida (subir, alcanzar pico, bajar) sin volver a subir después de bajar.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y métodos como `slice()`, `every()`, `indexOf()`
- Algoritmos de detección de picos y tendencias en secuencias
- Operadores de comparación y lógica condicional compuesta
- Conceptos de secuencias monótonas (crecientes y decrecientes)

---

## 📖 Contexto

Comprueba que el trineo de Santa Claus hace una **parábola** al saltar entre ciudades. Recibe un array de alturas.

Para que sea válido: debe **subir**, llegar al punto más alto, y luego **bajar**. No puede volver a subir después de haber bajado.

Ejemplos:
```javascript
checkJump([1, 3, 8, 5, 2]) // true  ∧
checkJump([1, 7, 3, 5])    // false ∨∧
```

Diagrama para [1, 3, 8, 5, 2]:
```
    8
   / \
  3   5
 /     \
1       2
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `checkJump(heights)` que reciba un array de números representing alturas y devuelva true si la secuencia forma una parábola válida (estrictamente creciente hasta un pico, luego estrictamente decreciente), false en caso contrario.

### Función sugerida

```javascript
function checkJump(heights) {
  let peak = Math.max(...heights)
  let peakIdx = heights.indexOf(peak)
  let asc = heights.slice(0, peakIdx)
  let desc = heights.slice(peakIdx + 1)
  
  if (asc.length === 0 || desc.length === 0) return false
  
  const ascOk = asc.every((h, i) => i === 0 || h >= asc[i - 1])
  const descOk = desc.every((h, i) => i === 0 || h <= desc[i - 1])
  return ascOk && descOk && peak !== heights[heights.length - 1]
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de detección de picos en arreglos: encontrar máximos locales y globales
- Técnicas de validación de secuencias: verificar monotonicidad estricta o no estricta
- Problemas de montaña en arrays (mountain array) y sus variantes
- Patrones de diseño para funciones de validación de propiedades secuenciales
- Complejidad algorítmica: análisis de operaciones de búsqueda, slicing y validación
- Métodos de división y conquista aplicados a validación de propiedades de arrays

---

## 🔗 Retos similares
