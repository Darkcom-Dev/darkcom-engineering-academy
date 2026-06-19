---
tipo: reto
dificultad: 3
---

# 📐 Día 11 — Progreso Scrum

---

## 🎯 Objetivo

Calcular la fracción completada de una tarea dada la duración esperada y el tiempo trabajado, devolviendo la fracción en su mínima expresión.

---

## 📚 Conocimientos Previos

- Manipulación de strings y conversión de formatos de tiempo (hh:mm:ss a segundos)
- Algoritmos para calcular el máximo común divisor (MCD)
- Conceptos de fracciones y simplificación matemática
- Operaciones aritméticas básicas y división entera

---

## 📖 Contexto

Papá Noel usa Scrum. Le dicen la duración esperada de una tarea y cuánto tiempo llevan trabajando (formato `hh:mm:ss`). Devuelve la **fracción completada** en su mínima expresión.

Ejemplos:
```javascript
getCompleted('01:00:00', '03:00:00') // '1/3'
getCompleted('02:00:00', '04:00:00') // '1/2'
getCompleted('00:10:00', '01:00:00') // '1/6'
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `getCompleted(part, total)` que reciba dos strings en formato `hh:mm:ss` representing el tiempo trabajado y la duración total esperada, y devuelva la fracción completada en su mínima expresión como string "numerador/denominador".

### Función sugerida

```javascript
function getCompleted(part, total) {
  const toSeconds = t => t.split(':').reduce((s, v) => s * 60 + +v, 0)
  const a = toSeconds(part), b = toSeconds(total)
  const gcd = (x, y) => y === 0 ? x : gcd(y, x % y)
  const d = gcd(a, b)
  return `${a / d}/${b / d}`
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de cálculo del máximo común divisor (MCD): Euclides, algoritmos extendidos
- Técnicas de simplificación de fracciones y reducción a términos más simples
- Conversión de unidades de tiempo: horas, minutos, segundos a segundos totales
- Patrones de diseño para funciones de transformación y normalización de valores
- Complejidad algorítmica: análisis del algoritmo de Euclides y operaciones de división
- Métodos de manejo de precisión y redondeo en cálculos fraccionarios

---

## 🔗 Retos similares
