---
tipo: reto
dificultad: 3
---

# 💡 Día 9 — Luces de Navidad

---

## 🎯 Objetivo

Calcular el número mínimo de segundos necesarios para que todos los LEDs de una tira se enciendan, dado su comportamiento específico de cambio de estado cada 7 segundos.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y técnicas de iteración
- Algoritmos de conteo de secuencias consecutivas
- Conceptos de simulación de estados y transiciones
- Operadores aritméticos y lógicos básicos

---

## 📖 Contexto

Una empresa de luces navideñas tiene tiras LED (array de 0s y 1s). Cada **7 segundos** los LEDs cambian su estado según estas reglas:

- Si el LED está **apagado (0)**, se enciende si el LED de su izquierda estaba encendido.
- Si el LED está **encendido (1)**, se mantiene siempre encendido.

Calcula cuántos segundos deben pasar hasta que **todos los LEDs estén encendidos**.

Ejemplos:
```javascript
countTime([0, 0, 0, 1]) // 21 (3 cambios × 7s)
countTime([0, 0, 1, 0, 0]) // 28 (4 cambios × 7s)
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `countTime(leds)` que reciba un array de números (0s y 1s) representando el estado inicial de los LEDs y devuelva el número de segundos necesarios para que todos los LEDs estén encendidos.

### Función sugerida

```javascript
function countTime(leds) {
  const arr = [...leds, ...leds] // duplicar para detectar ciclos
  let maxZeros = 0, zeros = 0
  for (const led of arr) {
    if (led === 0) {
      zeros++
      maxZeros = Math.max(maxZeros, zeros)
    } else {
      zeros = 0
    }
  }
  return maxZeros * 7
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de detección de secuencias consecutivas: encontrar la mayor secuencia de ceros
- Técnicas de manejo de arrays circulares o duplicados para detectar patrones que cruzan límites
- Problemas de propagación de estados: cómo un estado afecta a elementos adyacentes en el tiempo
- Patrones de diseño para funciones de simulación de procesos discretos en el tiempo
- Complejidad algorítmica: análisis de soluciones lineales vs. aproximaciones más complejas
- Estrategias de transformación de problemas: convertir problemas de tiempo en problemas de conteo

---

## 🔗 Retos similares
