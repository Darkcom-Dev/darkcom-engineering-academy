---
tema: "Clases y Encapsulamiento"
leccion: 02
titulo: "Subclases"
---

# Subclases

**Módulo:** `Clases y Encapsulamiento` | **Lección:** 02

## 🧩 Código JavaScript

```javascript
class Deportista{
        constructor(nombre, apellido){
          this.nombre = nombre;
          this.apellido = apellido;
        }
      }

      class Futbolista extends Deportista{
        constructor(nombre, apellido, goles){
          super(nombre, apellido);
          this.goles = goles;
        }
      }
```


## 📊 Diagrama Conceptual

```mermaid
graph TD
    A[Clase] --> B[constructor()]
    A --> C[Métodos]
    A --> D[Getters / Setters]
    A --> E[Subclase: extends]
    E --> F[super() - llama al padre]
    A --> G[#propiedad - Privada]
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[Clases]]
- [[Getters y Setters]]
- [[Veterinaria]]
- [[Veterinaria]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Prototipos]]
- ➡️ Módulo siguiente: [[Manejo de Excepciones y JSON]]
- 🏠 Volver al [[Índice del Curso]]
