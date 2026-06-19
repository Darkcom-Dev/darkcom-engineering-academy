---
tipo: reto
dificultad: 8
---

# 🏔️ Día 20 — Viajes Retadores

---

## 🎯 Objetivo

Determinar la distribución óptima de equipos de renos para entregar regalos a diferentes países, respetando restricciones específicas de capacidad y ordenamiento.

---

## 📚 Conocimientos Previos

- Algoritmos de distribución y asignación de recursos
- Programación dinámica y técnicas de optimización
- Manipulación compleja de arrays y objetos
- Conceptos de restricciones y validación de soluciones
- Análisis de casos múltiples y combinación de resultados

---

## 📖 Contexto

Papá Noel se ha dado cuenta de que ni con la colaboración de todos los elfos va a poder entregar todos los regalos a tiempo. Por eso va a pedir ayuda a sus amigos de Autentia.

Desde Autentia nos han indicado que necesitan un programa para saber qué equipo de renos enviar a cada país. Hay diferentes tipos de renos y cada uno de ellos puede llevar un peso de regalos. Por ejemplo:

```javascript
const reindeerTypes = [
  { type: 'Nuclear', weightCapacity: 50 },
  { type: 'Electric', weightCapacity: 10 },
  { type: 'Gasoline', weightCapacity: 5 },
  { type: 'Diesel', weightCapacity: 1 }
]
```

En el listado de regalos que tiene Papá Noel se expresa cuánto pesa cada regalo y cuál es su país destino. El peso de los regalos siempre es un número natural. Por ejemplo:

```javascript
const gifts = [
  { country: 'Spain', weight: 30 },
  { country: 'Spain', weight: 7 },
  { country: 'France', weight: 17 }
]
```

Autentia nos comenta que, para que el equipo de renos a enviar a cada país sea óptimo, deberíamos:

1. Enviar el mayor número de renos posibles de mayor capacidad de carga
2. Aprovechar al máximo el peso que cada reno puede soportar
3. Los renos tienen un comportamiento extraño y no admiten que en el equipo haya más renos de un tipo que renos del siguiente tipo por orden descendente de capacidad de carga.

Ejemplo para Francia (17):
- No se mandarían diecisiete renos diésel (17 × 1) porque existen renos con mayor capacidad
- Tampoco se mandaría un reno nuclear (50) porque se estaría desaprovechando su capacidad
- La solución óptima: un reno eléctrico (10), uno gasolina (5) y dos diésel (2 × 1) = 17

Ejemplo para España (37):
- No se puede mandar tres eléctricos (3 × 10) + uno gasolina (5) + dos diésel (2 × 1) porque tendría más eléctricos que gasolina
- Tampoco dos eléctricos (2 × 10) + tres gasolina (3 × 5) + dos diésel (2 × 1) porque tendría más gasolina que diésel
- La solución óptima: dos eléctricos (2 × 10), dos gasolina (2 × 5) y siete diésel (7 × 1) = 34 + 10 + 7 = 37

La función debe devolver los renos ordenados por capacidad de carga de mayor a menor para cada país.

---

## 🧩 ¿Qué debes implementar?

Implementar una función `howManyReindeers(reindeerTypes, gifts)` que:
1. Reciba un array de objetos `reindeerTypes` donde cada objeto tiene `type` (string) y `weightCapacity` (número)
2. Reciba un array de objetos `gifts` donde cada objeto tiene `country` (string) y `weight` (número)
3. Agrupe los regalos por país y calcule el peso total por país
4. Para cada país, determine la distribución óptima de renos según las reglas especificadas
5. Devuelva un array de objetos, uno por cada país, donde cada objeto contiene:
   - `country`: el nombre del país
   - `reindeers`: un array de objetos, cada uno con `type` y `num` (número de renos de ese tipo)
6. Ordene los renos dentro de cada país por capacidad de carga de mayor a menor

### Función sugerida (estructura básica - la solución completa es más compleja)

```javascript
function howManyReindeers(reindeerTypes, gifts) {
  // 1. Agrupar regalos por país y sumar pesos
  const weightsByCountry = {};
  gifts.forEach(gift => {
    weightsByCountry[gift.country] = (weightsByCountry[gift.country] || 0) + gift.weight;
  });
  
  // 2. Ordenar tipos de reno por capacidad de capacidad (mayor a menor)
  const sortedTypes = [...reindeerTypes].sort((a, b) => b.weightCapacity - a.weightCapacity);
  
  // 3. Para cada país, calcular distribución óptima de renos
  const result = [];
  for (const [country, totalWeight] of Object.entries(weightsByCountry)) {
    const countryReindeers = [];
    
    // Implementar algoritmo complejo de distribución aquí
    // (Ver archivo JS original para la implementación completa)
    
    result.push({
      country: country,
      reindeers: countryReindeers
    });
  }
  
  return result;
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Algoritmos de distribución de recursos con restricciones jerárquicas
- Técnicas de programación dinámica aplicadas a problemas de asignación con límites
- Problemas de cambio de moneda (coin change) con restricciones adicionales
- Algoritmos de asignación greedy (codicioso) con validación de restricciones
- Técnicas de backtracking y poda para espacios de solución grandes
- Patrones de diseño para funciones de optimización con múltiples criterios
- Complejidad algorítmica: análisis de soluciones exponenciales vs. aproximaciones eficientes
- Manejo de casos edge: cuando no es posible satisfacer todas las restricciones
- Técnicas de memoización para evitar recálculos en problemas de programación dinámica

---

## 🔗 Retos similares
