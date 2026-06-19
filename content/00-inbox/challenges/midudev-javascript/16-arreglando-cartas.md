---
tipo: reto
dificultad: 4
---

# ✉️ Día 16 — Arreglando Cartas

---

## 🎯 Objetivo

Crear un programa que formatee cartas aplicando múltiples reglas de formato de texto.

---

## 📚 Conocimientos Previos

- Manipulación de strings y expresiones regulares en JavaScript
- Métodos de string como `trim()`, `replace()`, `test()`
- Conceptos de patrones de texto y sustitución
- Operaciones en cadena (chaining) de métodos de string

---

## 📖 Contexto

Papá Noel recibe cartas con problemas de formato. Crea un programa que:

- Elimine espacios al inicio y final
- Deje solo un espacio entre palabras
- Deje un espacio después de cada coma
- Quite espacios antes de coma o punto
- Las preguntas solo deben terminar con `?`
- Primera letra de cada oración en mayúscula
- `"Santa Claus"` en mayúscula siempre
- Punto al final si no tiene puntuación

---

## 🧩 ¿Qué debes implementar?

Implementar una función `fixLetter(letter)` que reciba una string representando una carta con problemas de formato y devuelva la misma carta aplicando todas las reglas de formato especificadas.

### Función sugerida

```javascript
function fixLetter(letter) {
  return letter
    .trim()
    .replace(/\s+/g, ' ')
    .replace(/\s*([,.\?])\s*/g, '$1 ')
    .replace(/\?+/g, '?')
    .replace(/([.!?])\s*(\w)/g, (_, p, w) => p + ' ' + w.toUpperCase())
    .replace(/santa claus/gi, 'Santa Claus')
    .replace(/^\w/, c => c.toUpperCase())
    .replace(/\s*$/, '')
    + (/[.!?]$/.test(letter.trim()) ? '' : '.')
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Expresiones regulares avanzadas: grupos de captura, referencias atrás, lookaheads/lookbehinds
- Técnicas de procesamiento de texto: normalización, limpieza y formateo
- Algoritmos de corrección de texto: espaciado, puntuación y capitalización
- Patrones de diseño para funciones de transformación de texto con múltiples reglas
- Complejidad algorítmica: análisis de operaciones de reemplazo secuencial vs. compilado
- Métodos de encadenamiento de operaciones: rendimiento y legibilidad en pipelines de transformación

---

## 🔗 Retos similares

