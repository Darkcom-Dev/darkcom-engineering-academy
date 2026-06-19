---
tipo: reto
dificultad: 4
---

# 🔺 Día 14 — El Mejor Camino

---

## 🎯 Objetivo

Encontrar el camino de menor tiempo desde la cima hasta la base de una pirámide, moviéndose solo en diagonal izquierda o derecha.

---

## 📚 Conocimientos Previos

- Manipulación de arrays bidimensionales y métodos como `reduceRight()`, `map()`
- Algoritmos de programación dinámica: bottom-up y top-down
- Conceptos de árboles y grafos: recorridos y selección de caminos óptimos
- Operaciones de mínimos y acumulación condicional

---

## 📖 Contexto

Santa Claus construye **pirámides de hielo**. Cada reno comienza en la cima y debe deslizarse hacia abajo en diagonal (izquierda o derecha) eligiendo el camino de **menor tiempo**.

Ejemplo:
```javascript
getOptimalPath([[0], [7, 4], [2, 4, 6]]) // 8 (0→4→4)
```

Diagrama:
```
    0
   / \
  7   4
 / \ / \
2   4   6
```

El camino óptimo es 0 → 4 → 4 = 8

---

## 🧩 ¿Qué debes implementar?

Implementar una función `getOptimalPath(path)` que reciba un array bidimensional representando una pirámide (donde cada nivel tiene un elemento más que el nivel anterior) y devuelva la suma mínima de un camino desde la cima hasta la base, moviéndose solo en diagonal izquierda o derecha.

### Función sugerida

```javascript
function getOptimalPath(path) {
  return path.reduceRight((prev, curr) => {
    return curr.map((val, i) => val + Math.min(prev[i], prev[i + 1]))
  })[0]
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de programación dinámica: triangulación, memoización y tabulación
- Problemas de camino mínimo en triangulos: mínimo path sum en triangle
- Técnicas de procesamiento bottom-up vs top-down en estructuras jerárquicas
- Patrones de diseño para funciones de reducción acumulativa con condiciones
- Complejidad algorítmica: análisis de tiempo O(n²) y espacio O(n) vs O(n²)
- Estrategias de optimización de espacio: reutilización de arrays vs. creación de nuevos

---

## 🔗 Retos similares
