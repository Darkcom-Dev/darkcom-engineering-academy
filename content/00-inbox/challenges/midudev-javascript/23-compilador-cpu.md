---
tipo: reto
dificultad: 5
---

# 🖥️ Día 23 — Compilador CPU

---

## 🎯 Objetivo

Simular una CPU de 8 bits con 8 registros que ejecuta un conjunto específico de instrucciones de ensamblaje.

---

## 📚 Conocimientos Previos

- Manipulación de arrays y manipulación a nivel de bits
- Conceptos de arquitecturas de CPU: registros, instrucciones, contador de programa
- Operaciones bit a bit y manejo de desbordamiento
- Parsing e interpretación de comandos de texto
- Conceptos de conjuntos de instrucciones y ciclos de ejecución

---

## 📖 Contexto

Simula una CPU de 8 bits con 8 registros (`V00`-`V07`). Los registros almacenan valores de 8 bits (0-255) con desbordamiento.

Instrucciones soportadas:
- `MOV Vxx,Vyy` - Copia el valor de Vxx a Vyy
- `MOV n,Vxx` - Asigna el valor constante n al registro Vxx
- `ADD Vxx,Vyy` - Suma el valor de Vyy a Vxx (con desbordamiento de 8 bits)
- `DEC Vxx` - Decrementa el valor de Vxx en 1 (con desbordamiento)
- `INC Vxx` - Incrementa el valor de Vxx en 1 (con desbordamiento)
- `JMP i` - Salta a la instrucción i si el registro V00 no es cero

Ejemplo:
```javascript
executeCommands(['MOV 5,V00', 'MOV 10,V01', 'DEC V00', 'ADD V00,V01'])
// [14, 10, 0, 0, 0, 0, 0, 0]
```

Explicación paso a paso:
1. MOV 5,V00 → V00 = 5
2. MOV 10,V01 → V01 = 10
3. DEC V00 → V00 = 4
4. ADD V00,V01 → V00 = 4 + 10 = 14
Resultado final: [14, 10, 0, 0, 0, 0, 0, 0]

---

## 🧩 ¿Qué debes implementar?

Implementar una función `executeCommands(commands)` que:
1. Reciba un array de strings `commands` donde cada string es una instrucción de ensamblaje
2. Simule la ejecución de estas instrucciones en una CPU de 8 bits con 8 registros
3. Devuelva el estado final de los 8 registros como un array de números

### Función sugerida

```javascript
function executeCommands(commands) {
  const reg = Array(8).fill(0)
  const ops = {
    MOV: (a, b) => { reg[b] = isNaN(a) ? reg[a] : +a },
    ADD: (a, b) => { reg[a] = (reg[a] + reg[b]) & 255 },
    DEC: a => { reg[a] = (reg[a] - 1 + 256) & 255 },
    INC: a => { reg[a] = (reg[a] + 1) & 255 },
    JMP: a => { if (reg[0] !== 0) i = +a - 1 },
  }
  for (let i = 0; i < commands.length; i++) {
    const [op, args] = commands[i].split(' ')
    const params = args.split(',').map(p => p.startsWith('V') ? +p.slice(1) : p)
    ops[op](...params)
  }
  return reg
}
```

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

- Arquitecturas de CPU simples: registros, acumulador, contador de programa
- Técnicas de interpretación de lenguajes: ciclo fetch-decode-execute
- Manejo de desbordamiento en operaciones aritméticas: operaciones módulo y máscara de bits
- Patrones de diseño para intérpretes de lenguajes pequeños: comando patrón, tabla de operaciones
- Complejidad algorítmica: análisis de tiempo lineal en relación al número de instrucciones
- Estrategias de manejo de saltos condicionales: modificación segura del contador de programa
- Representación de números con complemento a dos vs. magnitud y signo (en este caso, magnitud pura con desbordamiento)

---

## 🔗 Retos similares
