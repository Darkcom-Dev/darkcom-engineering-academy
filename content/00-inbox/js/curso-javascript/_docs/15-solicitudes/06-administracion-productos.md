---
tema: "Solicitudes HTTP"
leccion: 06
titulo: "Cell-Land"
---

# Cell-Land

**Módulo:** `Solicitudes HTTP` | **Lección:** 06

Cell-Land

---

Administración de productos ID Nombre Modelo Color Almacenamiento Procesador

Consultar artículo Consultar Nombre: Modelo: Color: Almacenamiento: Procesador: Modificar Eliminar

Agregar artículo Agregar Nuevo

## 🧩 Código JavaScript

**Archivo:** `https://unpkg.com/axios@1.3.4/dist/axios.min.js`

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
- [[Otras Solicitudes]]
- [[Fetch Avanzado]]
- [[Axios en Detalle]]
- [[Tienda de telefonos]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Frameworks y Librerías]]
- ➡️ Módulo siguiente: [[Bases de Datos]]
- 🏠 Volver al [[Índice del Curso]]
