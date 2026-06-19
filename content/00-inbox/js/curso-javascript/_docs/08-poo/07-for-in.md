---
tema: "POO - Objetos"
leccion: 07
titulo: "Loop For In"
---

# Loop For In

**Módulo:** `POO - Objetos` | **Lección:** 07

## 🧩 Código JavaScript

```javascript
function Perro(raza, edad, color) {
            this.patas = 4;
            this.raza = raza;
            this.edad = edad;
            this.color = color;
            this.ladrar = function () {
                console.log("Guau");
            };
        };

        let simba = new Perro("Shih Tzu", 4, "marron");
        let reina = new Perro("Caniche", 15, "blanco");
        let batu = new Perro("Cruza", 8, "negro");

        for(let caracteristica in batu) {
          console.log(caracteristica + ": " + batu[caracteristica]);
        }

        let fede =  {};
```


## 📊 Diagrama Conceptual

```mermaid
graph TD
    A[Objeto JS] --> B[Propiedades: clave: valor]
    A --> C[Métodos: función()]
    B --> D[Acceso: obj.prop]
    B --> E[Acceso: obj['prop']]
    C --> F[this se refiere al objeto]
    A --> G[Se crean con {}]
```

```mermaid
graph LR
    A[Función Constructora] --> B[function Perro(){}]
    B --> C[new Perro()]
    C --> D[Instancia 1]
    C --> E[Instancia 2]
    B --> F[this.propiedad]
    B --> G[this.método]
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[Mi Primer Objeto]]
- [[Modificar Objetos]]
- [[Palabra Clave This]]
- [[Cajero automático]]
- [[Constructor]]
- [[Parámetros de Constructor]]
- [[Otras Formas de Crear Objetos]]
- [[Pakiman]]
- [[Empleados]]
- [[Registro de Empleados]]
- [[Villa platzi]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Proyecto: Tienda de Donas]]
- ➡️ Módulo siguiente: [[Prototipos]]
- 🏠 Volver al [[Índice del Curso]]
