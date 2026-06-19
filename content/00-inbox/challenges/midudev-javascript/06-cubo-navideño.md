---
tipo: reto
dificultad: 3
---

# 🔷 Día 6 — Cubo Navideño

---

## 🎯 Objetivo

Crear un programa que dibuje un cubo 3D en ASCII dado un tamaño específico, devolviendo un string con el diseño.

---

## 📚 Conocimientos Previos

- Manipulación de strings y saltos de línea (\n)
- Bucles for y acumulación de strings
- Métodos de string como repeat() y slice()
- Conceptos de geometría básica y proyección 3D a 2D

---

## 📖 Contexto

Crea un programa que dibuje un **cubo 3D** en ASCII. Se le pasa un número con el tamaño deseado y devuelve un string con el diseño.

Ejemplo para tamaño 3:
```
  /\_\_\
 /\/\_\_\
/\/\/\_\_\
\/\/\/_/_/_/
 \/\/_/_/_/
  \/_/_/_/
```

---

## 🧩 ¿Qué debes implementar?

Implementar una función `createCube(size)` que reciba un número entero positivo representing el tamaño del cubo y devuelva un string con el dibujo ASCII del cubo 3D.

### Función sugerida

```javascript
function createCube(size) {
  let top = '', bottom = ''
  for (let i = 0; i < size; i++) {
    top += ' '.repeat(size - i - 1)
    top += '/\\'.repeat(i + 1)
    top += '_\\'.repeat(size) + '\n'

    bottom += ' '.repeat(i)
    bottom += '\\/'.repeat(size - i)
    bottom += '_/'.repeat(size) + '\n'
  }
  return (top + bottom).slice(0, -1)
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de generación de arte ASCII: patrones repetitivos y simétricos
- Técnicas de construcción de strings: acumulación vs. plantillas
- Geometría computacional básica: proyección de 3D a 2D
- Patrones de diseño para funciones de generación de patrones visuales
- Análisis de complejidad: tiempo y espacio en relación al tamaño del cubo
- Métodos de manejo de saltos de línea y formato de salida multi-línea

---

## 🧪 Pruebas

```javascript
createCube(3)
```

---

## 🔗 Retos similares
