---
tipo: reto
dificultad: 1
---

# 🎁 Día 1 — Envolviendo Regalos

---

## 🎯 Objetivo

Crear un algoritmo que envuelva regalos con el símbolo `*` rodeándolos completamente por todos los lados.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y strings en JavaScript
- Métodos de array como `map()`, `repeat()`
- Plantillas literales (template literals) para construcción de strings
- Conceptos básicos de algoritmos de transformación de datos

---

## 📖 Contexto

Los elfos compraron una máquina que envuelve regalos... ¡pero no viene programada! Necesitamos crear un algoritmo que le ayude.

A la máquina se le pasa un array con los regalos (strings). Cada regalo debe envolverse con el símbolo `*` rodeándolo completamente por todos los lados.

```js
const gifts = ['book', 'game', 'socks']
const wrapped = wrapping(gifts)
console.log(wrapped)
/* [
     "******\n*book*\n******",
     "******\n*game*\n******",
     "*******\n*socks*\n*******"
   ] */
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `wrapping(gifts)` que reciba un array de strings y devuelva un array con cada string envuelto por el símbolo `*` en todos los lados.

### Función sugerida

```javascript
function wrapping(gifts) {
  return gifts.map(gift => {
    const top = '*'.repeat(gift.length + 2)
    return `${top}\n*${gift}*\n${top}`
  })
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de transformación de strings: padding, wrapping, encapsulation
- Técnicas de manipulación de arrays: map, filter, reduce
- Métodos de string en JavaScript: repeat(), concat(), plantillas literales
- Patrones de diseño para funciones puras y transformación de datos
- Complejidad algorítmica: análisis de tiempo y espacio en operaciones de mapeo

---

## 🔗 Retos similares

