---
tipo: reto
dificultad: 3
---

# 💡 Día 22 — Iluminación en Sintonía

---

## 🎯 Objetivo

Verificar que todas las secuencias de los sistemas de iluminación estén en orden estrictamente creciente.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y objetos en JavaScript
- Bucles for tradicionales y acceso por índice
- Operadores de comparación y condicionales
- Conceptos de mapeo y seguimiento de estado

---

## 📖 Contexto

Verifica que todas las secuencias de los sistemas de iluminación estén en orden **estrictamente creciente**.

Se le dan dos arrays:
- `systemNames`: array de strings que representan los nombres de los sistemas
- `stepNumbers`: array de números que representan los pasos/niveles de cada sistema

La función debe retornar `true` si para cada sistema, sus números de paso aparecen en orden estrictamente creciente en el array, y `false` en caso contrario.

Ejemplo:
```javascript
checkStepNumbers(
  ["tree_1", "tree_2", "house", "tree_1", "tree_2", "house"],
  [1, 33, 10, 2, 44, 20]
) // true
```

Explicación:
- tree_1: aparece en posiciones 0 y 3 con valores 1 y 2 → 1 < 2 ✓
- tree_2: aparece en posiciones 1 y 4 con valores 33 y 44 → 33 < 44 ✓
- house: aparece en posiciones 2 y 5 con valores 10 y 20 → 10 < 20 ✓
- Todos están en orden estrictamente creciente → retorna true

---

## 🧩 ¿Qué debes implementar?

Implementar una función `checkStepNumbers(systemNames, stepNumbers)` que:
1. Reciba un array de strings `systemNames` representando los nombres de los sistemas de iluminación
2. Reciba un array de números `stepNumbers` representando los pasos/niveles de cada sistema
3. Devuelva `true` si para cada sistema único, sus números de paso aparecen en orden estrictamente creciente en los arrays
4. Devuelva `false` si algún sistema tiene sus números de paso en orden no estrictamente creciente

### Función sugerida

```javascript
function checkStepNumbers(systemNames, stepNumbers) {
  const steps = {}
  for (let i = 0; i < systemNames.length; i++) {
    const name = systemNames[i]
    const step = stepNumbers[i]
    if (steps[name] !== undefined && steps[name] >= step) return false
    steps[name] = step
  }
  return true
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de validación de secuencias: verificar orden creciente o decreciente en datos
- Técnicas de seguimiento de estado: uso de objetos o maps para mantener el último valor visto
- Problemas de detección de duplicados con condiciones: validar propiedades en grupos
- Patrones de diseño para funciones de validación de propiedades secuenciales
- Complejidad algorítmica: análisis de tiempo lineal vs. enfoques más complejos
- Estrategias de terminación temprana: retornar false tan pronto como se detecta una violación
- Manejo de casos edge: arrays vacíos, elementos únicos, valores iguales (que violan el orden estrictamente creciente)

---

## 🔗 Retos similares
