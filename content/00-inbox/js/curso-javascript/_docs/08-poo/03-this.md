---
tema: "POO - Objetos"
leccion: 03
titulo: "Palabra Clave This"
---

# Palabra Clave "This"

**Módulo:** `POO - Objetos` | **Lección:** 03

## 🧩 Código JavaScript

```javascript
let perro = {
            nombre: "Simba",
            raza: "Shih Tzu",
            edad: 4,
            ladrar() {
                console.log("Guau");
            },
            saludar() {
                console.log("Hola, me llamo " + this.nombre);
            }
        };

        let perro2 = {
            nombre: "Reina",
            raza: "Caniche",
            edad: 15,
            ladrar() {
                console.log("Guau");
            },
            saludar() {
                console.log("Hola, me llamo " + this.nombre);
            }
        };
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
- [[Cajero automático]]
- [[Constructor]]
- [[Parámetros de Constructor]]
- [[Otras Formas de Crear Objetos]]
- [[Loop For In]]
- [[Pakiman]]
- [[Empleados]]
- [[Registro de Empleados]]
- [[Villa platzi]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Proyecto: Tienda de Donas]]
- ➡️ Módulo siguiente: [[Prototipos]]
- 🏠 Volver al [[Índice del Curso]]
