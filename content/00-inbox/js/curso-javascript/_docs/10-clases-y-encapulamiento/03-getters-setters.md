---
tema: "Clases y Encapsulamiento"
leccion: 03
titulo: "Getters y Setters"
---

# Getters y Setters

**Módulo:** `Clases y Encapsulamiento` | **Lección:** 03

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
          this._goles = goles;
        }

        get goles(){
          return this._goles;
        }

        set goles(nuevoGoles){
          this._goles = nuevoGoles;
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
- [[Subclases]]
- [[Veterinaria]]
- [[Veterinaria]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Prototipos]]
- ➡️ Módulo siguiente: [[Manejo de Excepciones y JSON]]
- 🏠 Volver al [[Índice del Curso]]
