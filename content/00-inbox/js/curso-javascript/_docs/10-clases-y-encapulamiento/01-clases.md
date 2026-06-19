---
tema: "Clases y Encapsulamiento"
leccion: 01
titulo: "Clases"
---

# Clases

**Módulo:** `Clases y Encapsulamiento` | **Lección:** 01

## 🧩 Código JavaScript

```javascript
class Papel{
            constructor(alto, ancho){
                this.alto = alto;
                this.ancho = ancho;
            }
        }

        let PapelA = class{
            constructor(alto, ancho){
                this.alto = alto;
                this.ancho = ancho;
            }
        }

        let PapelB = class PapelX{
            constructor(alto, ancho){
                this.alto = alto;
                this.ancho = ancho;
            }
        }

        let papelZ = new Papel(5,3);
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
- [[Subclases]]
- [[Getters y Setters]]
- [[Veterinaria]]
- [[Veterinaria]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Prototipos]]
- ➡️ Módulo siguiente: [[Manejo de Excepciones y JSON]]
- 🏠 Volver al [[Índice del Curso]]
