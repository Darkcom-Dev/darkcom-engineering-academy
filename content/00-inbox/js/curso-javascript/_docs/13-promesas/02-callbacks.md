---
tema: "Promesas y Async/Await"
leccion: 02
titulo: "Callbacks"
---

# Callbacks

**Módulo:** `Promesas y Async/Await` | **Lección:** 02

## 🧩 Código JavaScript

```javascript
function avanzaFila(callback) {
        setTimeout(function() {
          console.log("Tu turno ha llegado");
          callback();
          }, 5000);
      }

      function mujerTeLlama() {
        console.log("Te presentas a tu turno");
      }

      console.log("Llegas a la fila");
      avanzaFila(mujerTeLlama);
      console.log("Te vas a comprar cafe");
```


## 📊 Diagrama Conceptual

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

```mermaid
flowchart TD
    A[función async] --> B[await petición1]
    B --> C[Procesa resultado 1]
    C --> D[await petición2]
    D --> E[Procesa resultado 2]
    E --> F[Código secuencial]
    B -.->|Mientras espera| G[Event Loop sigue libre]
```

```mermaid
sequenceDiagram
    participant JS as JavaScript
    participant API as Servidor API
    JS->>API: fetch(url)
    API-->>JS: Response (Promise)
    JS->>JS: .then(res => res.json())
    JS->>JS: .then(data => {...})
    Note over JS: async/await también<br/>let res = await fetch(url)
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[Sincronía y Asincronpia]]
- [[Promesas]]
- [[Async/Await]]
- [[Manejo de Errores]]
- [[Cotizaciones]]
- [[Cotizaciones BTC]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Eventos]]
- ➡️ Módulo siguiente: [[Frameworks y Librerías]]
- 🏠 Volver al [[Índice del Curso]]
