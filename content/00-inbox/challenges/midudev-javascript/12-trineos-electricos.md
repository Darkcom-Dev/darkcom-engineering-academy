---
tipo: reto
dificultad: 2
---

# ⚡ Día 12 — Trineos Eléctricos

---

## 🎯 Objetivo

Determinar el mejor trineo eléctrico que pueda completar una distancia dada con su límite de batería.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y objetos en JavaScript
- Bucles for...of y acceso a propiedades de objetos
- Operaciones aritméticas básicas y comparaciones
- Conceptos de selección óptima basada en criterios

---

## 📖 Contexto

Papá Noel tiene nuevos trineos eléctricos con batería de **20W**. Cada trineo tiene `name` y `consumption` (vatios por unidad de distancia). Devuelve el nombre del **mejor trineo** que pueda recorrer la distancia con 20W.

Ejemplo:
```javascript
const sleighs = [
  { name: "Dasher", consumption: 0.3 },
  { name: "Dancer", consumption: 0.5 },
  { name: "Rudolph", consumption: 0.7 },
  { name: "Midu", consumption: 1 }
]
selectSleigh(30, sleighs) // "Dancer"
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `selectSleigh(distance, sleighs)` que reciba una distancia (número) y un array de objetos representando trineos (cada uno con `name` y `consumption`), y devuelva el nombre del trineo que pueda completar la distancia sin exceder los 20W de batería. Si ningún trineo puede completar la distancia, devolver null.

### Función sugerida

```javascript
function selectSleigh(distance, sleighs) {
  let best = null
  for (const s of sleighs) {
    if (s.consumption * distance <= 20) best = s.name
  }
  return best
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de selección y filtrado: encontrar el mejor candidato según múltiples criterios
- Técnicas de iteración y acumulación en arrays: bucles vs. métodos funcionales
- Problemas de asignación de recursos: asignar tareas a agentes según capacidades
- Patrones de diseño para funciones de selección óptima
- Complejidad algorítmica: análisis de soluciones lineales vs. más complejas
- Estrategias de manejo de casos edge: cuando ningún elemento cumple los criterios

---

## 🔗 Retos similares
