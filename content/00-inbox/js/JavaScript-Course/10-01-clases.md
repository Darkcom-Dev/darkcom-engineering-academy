# Lección 01: Clases en ES6

A partir de la versión ECMAScript 2015 (ES6), JavaScript introdujo la palabra clave `class`. Aunque por debajo sigue funcionando con prototipos, las clases ofrecen una sintaxis más limpia y familiar para programadores de otros lenguajes.

## Estructura de una Clase
Una clase se define con la palabra clave `class` seguida del nombre (en Mayúscula) y un bloque que contiene el `constructor`.

```javascript
class Papel {
    // El constructor inicializa las propiedades al crear el objeto
    constructor(alto, ancho) {
        this.alto = alto;
        this.ancho = ancho;
    }
}

// Instanciación
let miHoja = new Papel(297, 210);
```

## Formas de declarar Clases
Al igual que las funciones, las clases pueden declararse de varias maneras:

### 1. Declaración de Clase
Es la forma más común y recomendada.
```javascript
class Persona { ... }
```

### 2. Expresión de Clase (Anónima)
La clase se asigna a una variable.
```javascript
let PapelA = class {
    constructor(alto, ancho) { ... }
};
```

### 3. Expresión de Clase (Nombrada)
Útil para depuración, aunque el nombre interno (`PapelX`) solo es visible dentro de la clase.
```javascript
let PapelB = class PapelX { ... };
```

## Diferencias con Prototipos
| Característica | Prototipos / Constructores | Clases (ES6) |
| :--- | :--- | :--- |
| **Sintaxis** | `function Perro() { ... }` | `class Perro { ... }` |
| **Métodos** | `Perro.prototype.ladrar = ...` | Se definen dentro de la clase |
| **Hoisting** | Soportado | No soportado (debes declarar antes de usar) |

---
[[09-04-registro-automotor|<- Anterior]] | [[00-indice-curso|Índice]] | [[10-02-subclases|Siguiente ->]]
