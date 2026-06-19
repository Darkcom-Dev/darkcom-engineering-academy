---
tipo: reto
dificultad: 2
---

# 💰 Día 2 — Horas Extra

---

## 🎯 Objetivo

Calcular el número de horas extra que se deben trabajar en un año basado en los días festivos que caen entre lunes y viernes.

---

## 📚 Conocimientos Previos

- Manipulación de fechas en JavaScript (objeto Date)
- Métodos de array como `reduce()`
- Operadores condicionales y lógicos
- Conceptos básicos de algoritmos de acumulación

---

## 📖 Contexto

Un millonario ha comprado una red social y ha anunciado que cada vez que una jornada de trabajo se pierde por un día festivo, habrá que compensarlo con **2 horas extra** otro día del mismo año.

Dado un año y un array con las fechas de los días festivos (formato `MM/DD`), devuelve el número de horas extra que se harían ese año.

> Solo cuentan los festivos que caen en **lunes a viernes** (días laborables).

---

## 🧩 ¿Qué debes implementar?

Implementar una función `countHours(year, holidays)` que reciba un año (número) y un array de strings con formato `MM/DD` representing holidays, y devuelva el total de horas extra a trabajar.

### Función sugerida

```javascript
function countHours(year, holidays) {
  return holidays.reduce((hours, holiday) => {
    const date = new Date(`${year}/${holiday}`)
    const day = date.getDay()
    return day > 0 && day < 6 ? hours + 2 : hours
  }, 0)
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de filtrado y acumulación: filter + sum, reduce with conditions
- Trabajo con fechas y calendarios: determinación de día de la semana
- Técnicas de validación de rangos: validar valores dentro de límites específicos
- Patrones de diseño para funciones de agregación condicional
- Complejidad temporal: análisis de eficiencia en procesamiento de listas

---

## 🧪 Pruebas

```javascript
countHours(2022, ['01/06', '04/01', '12/25']) // 4
```

---

## 🔗 Retos similares

