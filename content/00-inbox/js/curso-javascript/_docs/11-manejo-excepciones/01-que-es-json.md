---
tema: "Manejo de Excepciones y JSON"
leccion: 01
titulo: "JSON"
---

# JSON

**Módulo:** `Manejo de Excepciones y JSON` | **Lección:** 01

## 🧩 Código JavaScript

```javascript
let datosJson;
        let xhr = new XMLHttpRequest();
        xhr.open('GET', "persona.json", true);
        xhr.responseType = 'json';
        xhr.onload = function() {
            if(xhr.status === 200) {
                datosJson = xhr.response;
                let elementoTexto = document.getElementById("nombre");
                elementoTexto.textContent = datosJson.nombre;
            } else {
                // manejar el error
            }
        }
        xhr.send();
```


## 📊 Diagrama Conceptual

```mermaid
graph LR
    A[JSON] --> B[JavaScript Object Notation]
    B --> C[Formato de intercambio]
    C --> D[fetch() - Obtener]
    C --> E[JSON.stringify() - Enviar]
    C --> F[JSON.parse() - Leer]
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

```mermaid
flowchart TD
    A[Código] --> B[try]
    B --> C[Código que puede fallar]
    C -->|Error| D[catch(error)]
    C -->|Éxito| E[Sigue normal]
    D --> F[finally - Siempre se ejecuta]
    E --> F
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[JSON]]
- [[JSON]]
- [[Resumen Cuenta Bancaria]]
- [[Banco Springfield]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Clases y Encapsulamiento]]
- ➡️ Módulo siguiente: [[Eventos]]
- 🏠 Volver al [[Índice del Curso]]
