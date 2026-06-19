---
tipo: reto
dificultad: 2
---

# 🦌 Día 3 — Distribuyendo Regalos

---

## 🎯 Objetivo

Calcular el número máximo de cajas que Santa puede entregar dadas las restricciones de peso de los regalos y capacidad de los renos.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y strings en JavaScript
- Métodos de array como `reduce()` para sumar propiedades
- Operaciones aritméticas básicas y división entera
- Conceptos de optimización y asignación de recursos

---

## 📖 Contexto

Santa Claus tiene una caja de regalos y una lista de renos. Cada regalo pesa **el número de letras de su nombre**. Cada reno puede cargar **2 × el número de letras de su nombre**.

Implementa una función que devuelva el **número máximo de cajas** que Santa puede entregar. Las cajas **no se pueden dividir**.

Ejemplo:
- Regalos: `["book", "doll", "ball"]` (pesos: 4+4+4 = 12)
- Renos: `["dasher", "dancer"]` (capacidades: (2*6)+(2*6) = 24)
- Resultado: `distributeGifts(packOfGifts, reindeers) // 2`

---

## 🧩 ¿Qué debes implementar?

Implementar una función `distributeGifts(packOfGifts, reindeers)` que:
1. Calcule el peso total de los regalos (suma de longitudes de todos los nombres)
2. Calcule la capacidad total de los renos (suma de 2 × longitud de cada nombre de reno)
3. Devuelva el número máximo de cajas completas que se pueden entregar (división entera)

### Función sugerida

```javascript
function distributeGifts(packOfGifts, reindeers) {
  const packWeight = packOfGifts.reduce((w, g) => w + g.length, 0)
  const reindeerCapacity = reindeers.reduce((c, r) => c + r.length * 2, 0)
  return Math.floor(reindeerCapacity / packWeight)
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de asignación de recursos: bin packing, knapsack problem
- Técnicas de cálculo de capacidades y limites en sistemas de distribución
- Métodos de redondeo y división entera en programación
- Patrones de diseño para funciones de cálculo de recursos y capacidades
- Análisis de complejidad: tiempo lineal vs constantes en operaciones de reducción

---

## 🧪 Pruebas

```javascript
distributeGifts(['book','doll','ball'], ['dasher','dancer']) // 2
```

---

## 🔗 Retos similares
