---
tipo: reto
dificultad: 4
---

# 🛷 Día 5 — Optimizando Viajes

---

## 🎯 Objetivo

Encontrar el número máximo de regalos que se pueden entregar haciendo el menor número posible de viajes, dado un límite de regalos por saco y un máximo de ciudades a visitar.

---

## 📚 Conocimientos Previos

- Generación de combinaciones y subconjuntos
- Algoritmos de fuerza bruta y optimización
- Manipulación de arrays y operaciones de reducción (sum)
- Conceptos de problemas de mochila (knapsack) y selección óptima

---

## 📖 Contexto

Papá Noel quiere dejar el máximo número de regalos haciendo el **menor número posible de viajes**. Recibe un array de regalos por ciudad, un límite de regalos en el saco, y un máximo de ciudades a visitar.

Si no puede dejar **todos** los regalos de una ciudad, no deja ninguno allí.

Ejemplo:
```javascript
const giftsCities = [12, 3, 11, 5, 7]
const maxGifts = 20, maxCities = 3
getMaxGifts(giftsCities, maxGifts, maxCities) // 20
// [12, 3, 5] suman 20, visitando 3 ciudades
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `getMaxGifts(giftsCities, maxGifts, maxCities)` que:
1. Genere todas las posibles combinaciones de ciudades (desde 1 hasta maxCities ciudades)
2. Para cada combinación, calcule la suma total de regalos
3. Devuelva la suma máxima que no exceda maxGifts

### Función sugerida

```javascript
function getMaxGifts(giftsCities, maxGifts, maxCities) {
  const combinations = (arr, k) => {
    if (k === 0) return [[]]
    if (arr.length === 0) return []
    const [first, ...rest] = arr
    const withFirst = combinations(rest, k - 1).map(c => [first, ...c])
    const withoutFirst = combinations(rest, k)
    return [...withFirst, ...withoutFirst]
  }

  let best = 0
  for (let k = 1; k <= maxCities; k++) {
    for (const combo of combinations(giftsCities, k)) {
      const sum = combo.reduce((a, b) => a + b, 0)
      if (sum <= maxGifts && sum > best) best = sum
    }
  }
  return best
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de generación de combinaciones: métodos iterativos y recursivos
- Problemas de mochila (knapsack) y sus variantes: 0/1 knapsack, unbounded knapsack
- Técnicas de poda y optimización en búsqueda exhaustiva
- Programación dinámica aplicada a problemas de selección
- Análisis de complejidad: tiempo exponencial vs. soluciones pseudo-polynomial
- Estrategias de búsqueda con backtracking y poda

---

## 🔗 Retos similares
