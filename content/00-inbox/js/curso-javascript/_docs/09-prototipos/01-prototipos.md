---
tema: "Prototipos"
leccion: 01
titulo: "Prototipos"
---

# Prototipos

**Módulo:** `Prototipos` | **Lección:** 01

## 🧩 Código JavaScript

```javascript
let perro = {nombre: 'simba'};
      let perro1 = Object.create(perro);
```


## 📊 Diagrama Conceptual

```mermaid
graph TD
    A[simba] --> B[Perro.prototype]
    B --> C[Object.prototype]
    C --> D[null]
    A -.->|hereda| B
    B -.->|hereda| C
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[Objetos Prototípicos]]
- [[Modificar Prototipos]]
- [[Registro Automotor]]
- [[Registro Automoviles]]

### Navegación del curso
- ⬅️ Módulo anterior: [[POO - Objetos]]
- ➡️ Módulo siguiente: [[Clases y Encapsulamiento]]
- 🏠 Volver al [[Índice del Curso]]
