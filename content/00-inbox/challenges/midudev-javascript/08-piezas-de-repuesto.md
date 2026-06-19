---
tipo: reto
dificultad: 3
---

# 🔧 Día 8 — Piezas de Repuesto

---

## 🎯 Objetivo

Determinar si una cadena puede convertirse en un palíndromo eliminando, como máximo, un carácter.

---

## 📚 Conocimientos Previos

- Manipulación de strings y métodos como `split()`, `reverse()`, `join()`
- Algoritmos de verificación de palíndromos
- Bucles for y acceso por índice a caracteres
- Conceptos de simetría y reflexión en secuencias

---

## 📖 Contexto

Las piezas de repuesto son cadenas de texto. Una pieza es válida si **puede ser un palíndromo después de eliminar, como máximo, un carácter**.

Ejemplos:
```javascript
checkPart("uwu")    // true — ya es palíndromo
checkPart("miidim") // true — quitando la 'i' queda "midim"
checkPart("midu")   // false — no se puede
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `checkPart(part)` que reciba una string y devuelva true si es posible convertirla en un palíndromo eliminando como máximo un carácter, false en caso contrario.

### Función sugerida

```javascript
function checkPart(part) {
  if (part === part.split('').reverse().join('')) return true
  for (let i = 0; i < part.length; i++) {
    const test = part.slice(0, i) + part.slice(i + 1)
    if (test === test.split('').reverse().join('')) return true
  }
  return false
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de verificación de palíndromos con tolerancia a errores
- Técnicas de comparación bidireccional (dos punteros) para palíndromos
- Problemas de edición de strings: distancia de edición mínima
- Patrones de diseño para funciones de validación con tolerancia a fallos
- Complejidad algorítmica: análisis de soluciones brute force vs optimizadas
- Estrategias de terminación temprana en bucles de validación

---

## 🔗 Retos similares
