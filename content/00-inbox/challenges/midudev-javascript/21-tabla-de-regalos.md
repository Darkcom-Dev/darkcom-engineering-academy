# 📊 Día 21 — Tabla de Regalos

Crea un programa que dibuje una **tabla ASCII** con los regalos y sus cantidades.

```js
printTable([
  { name: 'Game', quantity: 2 },
  { name: 'Bike', quantity: 1 },
  { name: 'Book', quantity: 3 }
])
// +++++++++++++++++++
// | Gift | Quantity |
// | ---- | -------- |
// | Game | 2        |
// | Bike | 1        |
// | Book | 3        |
// *******************
```

---

## 💡 Solución

```javascript
function printTable(gifts) {
  const maxName = Math.max(...gifts.map(g => g.name.length), 4)
  const maxQty = Math.max(...gifts.map(g => String(g.quantity).length), 8)
  const line = '+'.repeat(maxName + maxQty + 5)
  const sep = `| ${'-'.repeat(maxName)} | ${'-'.repeat(maxQty)} |`
  const header = `| Gift${' '.repeat(maxName - 4)} | Quantity${' '.repeat(maxQty - 8)} |`
  const bottom = '*'.repeat(maxName + maxQty + 5)
  const rows = gifts.map(g =>
    `| ${g.name}${' '.repeat(maxName - g.name.length)} | ${String(g.quantity)}${' '.repeat(maxQty - String(g.quantity).length)} |`
  )
  return [line, header, sep, ...rows, bottom].join('\n')
}
```

---

## 🔗 Retos similares
