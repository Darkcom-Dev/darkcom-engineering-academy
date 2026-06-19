---
tipo: reto
dificultad: 2
---

# 🎄 Día 15 — Decorando el Árbol

---

## 🎯 Objetivo

Determinar si es posible colocar un adorno en un árbol de Navidad sin que quede vacío ningún nivel del árbol.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y operaciones de acceso por índice
- Algoritmos de conteo y acumulación
- Conceptos de estructuras de árboles binarios y niveles
- Operaciones aritméticas básicas y comparaciones

---

## 📖 Contexto

Cada año, Papa Noel necesita decorar el árbol de Navidad con adornos. El árbol tiene una estructura triangular donde cada nivel tiene una capacidad específica de adornos.

Ejemplo de árbol con 4 niveles:
```
   🎄     <- Nivel 1: 1 posición
  🎄 🎄   <- Nivel 2: 2 posiciones
 🎄 🎄 🎄  <- Nivel 3: 3 posiciones
🎄 🎄 🎄 🎄 <- Nivel 4: 4 posiciones
```

Un adorno se coloca en una posición específica (fila, columna). Determinar si después de colocar todos los adornos, ningún nivel queda vacío.

---

## 🧩 ¿Qué debes implementar?

Implementar una función que reciba un array de posiciones donde se colocan los adornos y el número de niveles del árbol, y devuelva true si después de colocar todos los adornos, ningún nivel del árbol queda vacío, false en caso contrario.

### Ejemplo de función sugerida

```javascript
function decorarArbol(adornos, niveles) {
  // Inicializar un array para contar adornos por nivel
  const nivelesOcupados = new Array(niveles).fill(0)
  
  // Contar cuántos adornos se colocan en cada nivel
  for (const [fila, columna] of adornos) {
    if (fila >= 1 && fila <= niveles) {
      nivelesOcupados[fila - 1]++
    }
  }
  
  // Verificar que ningún nivel quede vacío (0 adornos)
  return nivelesOcupados.every(count => count > 0)
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de conteo por categorías: bucket counting o histogramas
- Técnicas de validación de cobertura: verificar que todos los elementos de un conjunto estén presentes
- Estructuras de datos para acumulación y conteo eficiente
- Patrones de diseño para funciones de validación de completitud
- Complejidad algorítmica: análisis de tiempo lineal vs. constantes
- Estrategias de manejo de índices basados en 1 vs basado en 0

---

## 🔗 Retos similares
