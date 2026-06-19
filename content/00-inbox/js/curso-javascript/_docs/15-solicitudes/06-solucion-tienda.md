---
tema: "Solicitudes HTTP"
leccion: 06
titulo: "Tienda de telefonos"
---

# Tienda de telefonos

**Módulo:** `Solicitudes HTTP` | **Lección:** 06

Tienda de teléfonos Id Marca Modelo Color Almacenamiento Procesador ID Marca Modelo Color Almacenamiento Procesador Obtener Registrar Borrar Actualizar

## 🧩 Código JavaScript

**Archivo:** `https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js`

**Archivo:** `main.js`


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
- [[Otras Solicitudes]]
- [[Fetch Avanzado]]
- [[Axios en Detalle]]
- [[Cell-Land]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Frameworks y Librerías]]
- ➡️ Módulo siguiente: [[Bases de Datos]]
- 🏠 Volver al [[Índice del Curso]]
