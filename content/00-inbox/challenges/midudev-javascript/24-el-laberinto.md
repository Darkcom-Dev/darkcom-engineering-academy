---
tipo: reto
dificultad: 4
---

# 🌀 Día 24 — El Laberinto

---

## 🎯 Objetivo

Determinar si es posible salir de un laberinto dado un punto de entrada y salida, moviéndose solo en las cuatro direcciones cardinales.

---

## 📚 Conocimientos Previos

- Algoritmos de búsqueda en grafos: BFS (Breadth-First Search) y DFS (Depth-First Search)
- Manipulación de matrices bidimensionales en JavaScript
- Uso de colas y conjuntos para recorrido de grafos
- Conceptos de visitado y evitación de ciclos en búsquedas

---

## 📖 Contexto

Indielfo Jones quiere saber si puede salir del laberinto. La matriz tiene:
- `W` = pared (no se puede pasar)
- `S` = punto de entrada
- `E` = punto de salida
- ` ` (espacio) = camino libre

Movimientos: arriba, abajo, izquierda, derecha (no diagonal).

---

## 🧩 ¿Qué debes implementar?

Implementar una función `canExit(maze)` que reciba una matriz representando el laberinto y devuelva true si es posible llegar desde el punto de entrada 'S' al punto de salida 'E' moviéndose solo arriba, abajo, izquierda o derecha a través de espacios (' '), false en caso contrario.

### Función sugerida

```javascript
function canExit(maze) {
  const rows = maze.length, cols = maze[0].length
  const visited = Array.from({ length: rows }, () => Array(cols).fill(false))
  const queue = []
  for (let r = 0; r < rows; r++)
    for (let c = 0; c < cols; c++)
      if (maze[r][c] === 'S') queue.push([r, c])
  while (queue.length) {
    const [r, c] = queue.shift()
    if (r < 0 || r >= rows || c < 0 || c >= cols) continue
    if (visited[r][c] || maze[r][c] === 'W') continue
    visited[r][c] = true
    if (maze[r][c] === 'E') return true
    queue.push([r - 1, c], [r + 1, c], [r, c - 1], [r, c + 1])
  }
  return false
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de búsqueda en grafos: BFS vs DFS y sus aplicaciones en laberintos
- Técnicas de evitación de ciclos en recorridos de grafos usando estructuras de datos de visitado
- Patrones de diseño para funciones de búsqueda de rutas en espacios discretos
- Complejidad algorítmica: análisis de tiempo y espacio en relación al tamaño del laberinto
- Estrategias de manejo de casos edge: laberintos vacíos, sin entrada/salida, completamente bloqueados

---

## 🔗 Retos similares
