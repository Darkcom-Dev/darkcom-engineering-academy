---
modulo: "Promesas y Async/Await"
---

# Promesas y Async/Await

## 📊 Diagrama del Módulo

```mermaid
stateDiagram-v2
    [*] --> Pending
    Pending --> Fulfilled: resolve()
    Pending --> Rejected: reject()
    Fulfilled --> [*]
    Rejected --> [*]
    Note right of Pending: Estado inicial
    
    state "then()" as THEN
    Fulfilled --> THEN
    Rejected --> catch()
```

## Lecciones

- [[Sincronía y Asincronpia]]
- [[Callbacks]]
- [[Promesas]]
- [[Async/Await]]
- [[Manejo de Errores]]
- [[Cotizaciones]]
- [[Cotizaciones BTC]]

---

⬅️ [[Eventos]] | [🏠 Índice del Curso](../index.md) | [[Frameworks y Librerías]] ➡️
