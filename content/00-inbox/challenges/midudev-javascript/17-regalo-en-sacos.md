---
tipo: reto
dificultad: 3
---

# 👜 Día 17 — Regalos en Sacos

---

## 🎯 Objetivo

Agrupar regalos en sacos respetando el orden original y el peso máximo de cada saco, donde el peso de cada regalo corresponde a la longitud de su nombre.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y strings en JavaScript
- Bucles for...of y condicionales
- Operaciones básicas de concatenación y medición de longitud
- Conceptos de acumulación y agrupamiento secuencial

---

## 📖 Contexto

Preparamos sacos para regalos. Cada saco tiene un peso máximo. El peso de cada regalo es la **longitud de su nombre**. Agrupa los regalos en sacos respetando el orden y el peso máximo.

Ejemplo:
```javascript
carryGifts(['game', 'bike', 'book', 'toy'], 10)
// ['game bike', 'book toy']
```

En este ejemplo:
- "game" (4) + "bike" (4) = 8 ≤ 10 → caben en el mismo saco
- "book" (4) + "toy" (4) = 8 ≤ 10 → caben en el mismo saco
- Pero si intentáramos poner "game" (4) + "bike" (4) + "book" (4) = 12 > 10 → excede el peso máximo

---

## 🧩 ¿Qué debes implementar?

Implementar una función `carryGifts(gifts, maxWeight)` que:
1. Reciba un array de strings `gifts` representando los nombres de los regalos
2. Reciba un número `maxWeight` representando el peso máximo que puede soportar cada saco
3. Agrupe los regalos en sacos siguiendo el orden original del array
4. Asegure que el peso total de los regalos en cada saco no exceda `maxWeight` (donde el peso de cada regalo es su longitud)
5. Devuelva un array de strings donde cada string representa un saco con sus regalos separados por espacios
6. Devuelva un array vacío si algún regalo individual excede el peso máximo del saco

### Función sugerida

```javascript
function carryGifts(gifts, maxWeight) {
  if (gifts.some(g => g.length > maxWeight)) return []
  const sacks = []
  let current = '', currentW = 0
  for (const gift of gifts) {
    if (currentW + gift.length <= maxWeight) {
      current += (current ? ' ' : '') + gift
      currentW += gift.length
    } else {
      sacks.push(current)
      current = gift
      currentW = gift.length
    }
  }
  if (current) sacks.push(current)
  return sacks
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de partición secuencial: dividir secuencias manteniendo el orden original
- Técnicas de empaquetado greedy (codicioso): llenar cada contenedor lo más posible antes de pasar al siguiente
- Problemas de asignación secuencial: asignar elementos en orden a contenedores con capacidad limitada
- Patrones de diseño para funciones de acumulación con límites
- Complejidad algorítmica: análisis de tiempo lineal vs. enfoques más complejos
- Estrategias de manejo de casos edge: elementos individuales que exceden la capacidad

---

## ⚠️ Advertencias

- Si algún regalo individual tiene una longitud mayor que `maxWeight`, la función debe devolver un array vacío
- El orden de los regalos debe mantenerse exactamente como aparece en el array de entrada
- Cada saco debe contener al menos un regalo (excepto cuando se devuelve array vacío)
- Los regalos dentro de cada saco deben estar separados por exactamente un espacio

---

## 🔗 Retos similares
