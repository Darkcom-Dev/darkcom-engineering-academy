---
tipo: reto
dificultad: 3
---

# 🧸 Día 19 — Ordenando Regalos

---

## 🎯 Objetivo

Ordenar un array de juguetes según sus posiciones especificadas en un segundo array.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y métodos como `map()` y `sort()`
- Uso de desestructuración de objetos y arrow functions
- Conceptos de ordenamiento basado en propiedades
- Técnicas de transformación de arrays mediante mapeo y filtrado

---

## 📖 Contexto

Papá Noel tiene el almacén hecho un desastre. Recibe un array de juguetes y otro de posiciones. Ordena los juguetes según su posición.

Ejemplo:
```javascript
sortToys(['ball', 'doll', 'car', 'puzzle'], [2, 3, 1, 0])
// ['puzzle', 'car', 'ball', 'doll']
```

En este ejemplo:
- 'ball' está en posición 2
- 'doll' está en posición 3
- 'car' está en posición 1
- 'puzzle' está en posición 0

Al ordenar por posición (0, 1, 2, 3) obtenemos: ['puzzle', 'car', 'ball', 'doll']

---

## 🧩 ¿Qué debes implementar?

Implementar una función `sortToys(toys, positions)` que:
1. Reciba un array de strings `toys` representando los nombres de los juguetes
2. Reciba un array de números `positions` representando la posición deseada de cada juguete
3. Devuelva un array con los juguetes ordenados según sus posiciones

### Función sugerida

```javascript
function sortToys(toys, positions) {
  return toys
    .map((toy, i) => ({ toy, pos: positions[i] }))
    .sort((a, b) => a.pos - b.pos)
    .map(({ toy }) => toy)
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de ordenamiento personalizado: comparadores y funciones de clave
- Técnicas de transformación de datos: emparejamiento, ordenamiento y separación
- Patrones de diseño para funciones de reordenamiento basado en índices externos
- Complejidad algorítmica: análisis de operaciones de mapeo y ordenamiento
- Métodos alternativos: crear un array de resultados y colocar elementos directamente en su posición
- Manejo de casos edge: arrays vacíos, posiciones duplicadas o fuera de rango

---

## 🔗 Retos similares
