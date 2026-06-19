---
tema: "Promesas y Async/Await"
leccion: 04
titulo: "Async/Await"
---

# Async/Await

**Módulo:** `Promesas y Async/Await` | **Lección:** 04

## 🧩 Código JavaScript

```javascript
async function obtenerTodo() {
        console.log("Este codigo esta al comienzo");
        let respuestaGasolina = await fetch('https://api.datos.gob.mx/v1/precio.gasolina.publico');
        let datosGasolina = await respuestaGasolina.json();
        
        console.log("Este codigo esta al medio");
        let respuestaDolar = await fetch('https://open.er-api.com/v6/latest/USD');
        let datosDolar = await respuestaDolar.json();
        
        console.log(datosGasolina, datosDolar);
        console.log("Este codigo esta al final");
      }

      obtenerTodo();
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
- [[Callbacks]]
- [[Promesas]]
- [[Manejo de Errores]]
- [[Cotizaciones]]
- [[Cotizaciones BTC]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Eventos]]
- ➡️ Módulo siguiente: [[Frameworks y Librerías]]
- 🏠 Volver al [[Índice del Curso]]
