---
tema: "Promesas y Async/Await"
leccion: 03
titulo: "Promesas"
---

# Promesas

**Módulo:** `Promesas y Async/Await` | **Lección:** 03

## 🧩 Código JavaScript

```javascript
function obtenerUsuarios() {
        return new Promise(function(resolve, reject) {
          let xhr = new XMLHttpRequest();
          xhr.open('GET', 'https://jsonplaceholder.typicode.com/users');
          xhr.onload = function() {
            if(xhr.status === 200) {
              resolve(JSON.parse(xhr.responseText));
            } else {
              reject(xhr.statusText);
            }
          }
          xhr.send();
        });
      }

      obtenerUsuarios()
        .then(function(usuarios) {
          console.log(usuarios);
        })
        .catch(function(error) {
          console.error(error);
        });
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
- [[Async/Await]]
- [[Manejo de Errores]]
- [[Cotizaciones]]
- [[Cotizaciones BTC]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Eventos]]
- ➡️ Módulo siguiente: [[Frameworks y Librerías]]
- 🏠 Volver al [[Índice del Curso]]
