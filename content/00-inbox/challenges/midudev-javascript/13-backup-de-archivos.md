---
tipo: reto
dificultad: 3
---

# 💾 Día 13 — Backup de Archivos

---

## 🎯 Objetivo

Identificar qué archivos necesitan ser respaldados en un sistema de backup incremental basado en timestamps.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y tuplas (arrays de 2 elementos)
- Uso de objetos Set para almacenamiento único y eficiente
- Operaciones de filtrado y transformación de arrays
- Conceptos de timestamps y comparación temporal

---

## 📖 Contexto

Papá Noel hace **backups incrementales**. Recibe el timestamp del último backup y un array de cambios `[id, timestamp]`. Devuelve los IDs de los archivos modificados **después** del último backup.

Ejemplo:
```javascript
const lastBackup = 1546300800
const changes = [
  [1, 1546300800], [2, 1546300800],
  [1, 1546300900], [1, 1546301000],
  [3, 1546301100]
]
getFilesToBackup(lastBackup, changes) // [1, 3]
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `getFilesToBackup(lastBackup, changes)` que:
1. Reciba un timestamp del último backup (número)
2. Reciba un array de cambios donde cada cambio es un array `[id, timestamp]`
3. Devuelva un array ordenado de forma ascendente con los IDs únicos de los archivos que tienen un timestamp mayor al del último backup

### Función sugerida

```javascript
function getFilesToBackup(lastBackup, changes) {
  const ids = new Set()
  for (const [id, time] of changes) {
    if (time > lastBackup) ids.add(id)
  }
  return [...ids].sort((a, b) => a - b)
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de filtrado y extracción de datos basados en condiciones temporales
- Técnicas de deduplicación y almacenamiento único: uso de Sets vs. objetos
- Patrones de diseño para funciones de procesamiento de eventos temporales
- Complejidad algorítmica: análisis de tiempo lineal vs. estructuras de datos más complejas
- Estrategias de manejo de datos temporales: timestamps, fechas y comparaciones
- Métodos de transformación de arrays: map, filter, reduce y sus combinaciones

---

## 🔗 Retos similares
