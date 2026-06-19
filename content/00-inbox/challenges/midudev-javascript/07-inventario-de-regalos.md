---
tipo: reto
dificultad: 2
---

# 📋 Día 7 — Inventario de Regalos

---

## 🎯 Objetivo

Identificar qué regalos necesitan reponerse cuando solo hay stock en uno de tres almacenes.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y conjuntos (Set) en JavaScript
- Métodos de array como `includes()`, `filter()`, y spread operator
- Conceptos de operaciones de conjuntos: unión, intersección, diferencia
- Algoritmos de conteo y frecuencia de elementos

---

## 📖 Contexto

En los almacenes de Papá Noel están haciendo inventario. Hay **tres almacenes** (arrays). Un regalo se debe reponer cuando **solo hay stock en uno de los tres almacenes**.

Ejemplo:
```javascript
const a1 = ['bici', 'coche', 'bici', 'bici']
const a2 = ['coche', 'bici', 'muñeca', 'coche']
const a3 = ['bici', 'pc', 'pc']
getGiftsToRefill(a1, a2, a3) // ['muñeca', 'pc']
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `getGiftsToRefill(a1, a2, a3)` que reciba tres arrays representando el inventario de tres almacenes y devuelva un array con los elementos que aparecen en exactamente uno de los tres arrays.

### Función sugerida

```javascript
function getGiftsToRefill(a1, a2, a3) {
  const all = new Set([...a1, ...a2, ...a3])
  return [...all].filter(gift => {
    let count = 0
    if (a1.includes(gift)) count++
    if (a2.includes(gift)) count++
    if (a3.includes(gift)) count++
    return count === 1
  })
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de frecuencia y conteo de elementos en múltiples arrays
- Técnicas de operaciones de conjuntos: diferencia simétrica y elementos únicos
- Patrones de diseño para funciones de análisis de pertenencia múltiple
- Complejidad algorítmica: optimización de búsquedas múltiples en arrays
- Estructuras de datos auxiliares: uso de objetos o maps para conteo eficiente
- Métodos funcionales de programación: reduce, map, filter combinados

---

## 🧪 Pruebas

```javascript
getGiftsToRefill(['bici', 'coche', 'bici', 'bici'], ['coche', 'bici', 'muñeca', 'coche'], ['bici', 'pc', 'pc']) // ['muñeca', 'pc']
```

---

## 🔗 Retos similares
