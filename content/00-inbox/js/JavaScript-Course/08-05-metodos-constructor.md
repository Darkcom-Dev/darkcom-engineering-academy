# Lección 05: Parámetros y Métodos en el Constructor

Los constructores no serían muy útiles si todos los objetos fueran exactamente iguales. Para personalizar cada instancia, pasamos **parámetros** a la función constructora.

## Constructores con Parámetros
Podemos recibir valores y asignarlos a las propiedades del objeto usando `this`.

```javascript
function Perro(raza, edad) {
    this.patas = 4; // Propiedad fija para todos
    this.raza = raza; // Propiedad personalizada
    this.edad = edad; // Propiedad personalizada
    
    this.ladrar = function () {
        console.log("Guau");
    };
};

// Creamos objetos únicos con el mismo molde
let simba = new Perro("Shih Tzu", 4);
let reina = new Perro("Caniche", 15);
```

## Ejemplo Avanzado: Empleado
Los métodos también pueden interactuar con las propiedades personalizadas para generar comportamientos únicos.

```javascript
function Empleado(nombre, apellido, edad, cargo) {
    this.nombre = nombre;
    this.apellido = apellido;
    this.edad = edad;
    this.cargo = cargo;
    
    this.presentarse = function () {
        console.log(`Mi nombre es ${this.nombre} ${this.apellido}, soy ${this.cargo}, y tengo ${this.edad} años.`);
    };
};

let empleado1 = new Empleado("Juan", "Pérez", 30, "Programador");
empleado1.presentarse(); 
// Salida: Mi nombre es Juan Pérez, soy Programador, y tengo 30 años.
```

### Resumen de Componentes
- **Instancia:** Cada objeto creado (ej: `simba`) es una "instancia" del constructor `Perro`.
- **Atributos:** Son las variables internas (`nombre`, `edad`).
- **Comportamiento:** Son las funciones internas (`presentarse`).

---
[[08-04-constructor|<- Anterior]] | [[00-indice-curso|Índice]] | [[08-06-objetos-literales|Siguiente ->]]
