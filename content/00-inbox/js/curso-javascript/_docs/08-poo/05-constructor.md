---
tema: "POO - Objetos"
leccion: 05
titulo: "Parámetros de Constructor"
---

# Parámetros de Constructor

**Módulo:** `POO - Objetos` | **Lección:** 05

## 🧩 Código JavaScript

```javascript
function Perro(raza, edad) {
            this.patas = 4;
            this.raza = raza;
            this.edad = edad;
            this.ladrar = function () {
                console.log("Guau");
            };
        };

        let simba = new Perro("Shih Tzu", 4);
        let reina = new Perro("Caniche", 15);
        let batu = new Perro("Cruza", 8);

        function Empleado(nombre, apellido, edad, cargo) {
          this.nombre = nombre;
          this.apellido = apellido;
          this.edad = edad;
          this.cargo = cargo;
          this.presentarse = function () {
            console.log(`Mi nombre es ${this.nombre} ${this.apellido}, soy ${this.cargo}, y tengo ${this.edad} años de edad`);
          };
        };
        
        let miArray = [1, 2, 3];
        for(propiedad in miArray)
        {console.log(propiedad + " - " + miArray[propiedad])}
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
