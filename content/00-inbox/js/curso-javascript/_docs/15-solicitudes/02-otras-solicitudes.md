---
tema: "Solicitudes HTTP"
leccion: 02
titulo: "Otras Solicitudes"
---

# Otras Solicitudes

**Módulo:** `Solicitudes HTTP` | **Lección:** 02

## 🧩 Código JavaScript

**Archivo:** `misScripts.js`


## 📊 Diagrama Conceptual

```mermaid
graph LR
    A[Métodos HTTP] --> B[GET - Obtener]
    A --> C[POST - Crear]
    A --> D[PUT - Actualizar]
    A --> E[PATCH - Modificar parcial]
    A --> F[DELETE - Eliminar]
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
- [[Fetch a Fondo]]
- [[Fetch Avanzado]]
- [[Axios en Detalle]]
- [[Cell-Land]]
- [[Tienda de telefonos]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Frameworks y Librerías]]
- ➡️ Módulo siguiente: [[Bases de Datos]]
- 🏠 Volver al [[Índice del Curso]]
