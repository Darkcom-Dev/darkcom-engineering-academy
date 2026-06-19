---
tema: "Promesas y Async/Await"
leccion: 05
titulo: "Manejo de Errores"
---

# Manejo de Errores

**Módulo:** `Promesas y Async/Await` | **Lección:** 05

## 🧩 Código JavaScript

```javascript
/*//CALLBACKS
      function sumarNumeros(a, b, callback) {
        setTimeout(function() {
          if(typeof a != 'number' || typeof b != 'number') {
            return callback(new Error('Algun argumento no es numero'));
          }
          callback(null, a+b);
        }, 1000); 
      }

      sumarNumeros('1', 2, function(error, resultado) {
        if (error) {
          console.error(error);
        } else {
          console.log(resultado);
        }
      });

      //PROMESAS
      function sumarNumeros(a, b) {
        return new Promise(function(resolve, reject) {
          setTimeout(function() {
            if(typeof a != 'number' || typeof b != 'number') {
              reject(new Error("Ambos argumentos deben ser numeros"));
            } else {
              resolve(a + b);
            }
          }, 1000);
        })
         
      }

      sumarNumeros(1, '2')
      .then(function(resultado) {
        console.log(resultado);
      }).catch(function(error) {
        console.error(error);
      })*/


      //ASYNC/AWAIT
      async function sumarNumeros(a, b) {
        if(typeof a != 'number' || typeof b != 'number') {
          throw new Error('Alguno de los argumentos no es numero');
        }
        return a + b
      }

      async function manejarErrores() {
        try{
          let resultado = await sumarNumeros('2', 3);
          console.log(resultado);
        } catch (error) {
          console.error(error.message);
        }
      }

      manejarErrores();
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
- [[Async/Await]]
- [[Cotizaciones]]
- [[Cotizaciones BTC]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Eventos]]
- ➡️ Módulo siguiente: [[Frameworks y Librerías]]
- 🏠 Volver al [[Índice del Curso]]
