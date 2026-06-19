---
tema: "Prototipos"
leccion: 03
titulo: "Modificar Prototipos"
---

# Modificar Prototipos

**Módulo:** `Prototipos` | **Lección:** 03

## 🧩 Código JavaScript

```javascript
function Libro(autor, titulo, cantPaginas){
        this.autor = autor;
        this.titulo = titulo;
        this.cantPaginas = cantPaginas;
      }

      let libro1 = new Libro("Stephen King", "Carrie", 524);

      Libro.prototype.abrirLibro = function(){
        alert(this.titulo + " ha sido abierto");
      }

      Libro.prototype.edicion = this.edicion;

      function UnObjeto(a) {
        this.a = a;
      }

      UnObjeto.prototype.metodo1 = function(){};
      UnObjeto.prototype.metodo2 = function() {};
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
- [[Prototipos]]
- [[Objetos Prototípicos]]
- [[Registro Automotor]]
- [[Registro Automoviles]]

### Navegación del curso
- ⬅️ Módulo anterior: [[POO - Objetos]]
- ➡️ Módulo siguiente: [[Clases y Encapsulamiento]]
- 🏠 Volver al [[Índice del Curso]]
