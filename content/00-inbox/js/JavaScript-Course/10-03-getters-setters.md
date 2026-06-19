# Lección 03: Getters y Setters

Los **Getters** y **Setters** son métodos especiales que nos permiten acceder y modificar las propiedades de un objeto como si fueran variables, pero con la capacidad de ejecutar lógica intermedia (validaciones, formateo, etc.).

## Definición y Sintaxis
Se utilizan las palabras clave `get` y `set`. Por convención, la propiedad interna se suele nombrar con un guion bajo (`_`) para diferenciarla del método.

```javascript
class Futbolista extends Deportista {
    constructor(nombre, apellido, goles) {
        super(nombre, apellido);
        this._goles = goles; // Propiedad "privada" por convención
    }

    // Getter: Obtener el valor
    get goles() {
        return this._goles;
    }

    // Setter: Modificar el valor
    set goles(nuevoValor) {
        if (nuevoValor >= 0) {
            this._goles = nuevoValor;
        } else {
            console.error("Los goles no pueden ser negativos");
        }
    }
}
```

## Ventajas del Encapsulamiento
1. **Control:** Podemos evitar que se asignen valores inválidos (ej: goles negativos).
2. **Abstracción:** El usuario de la clase no necesita saber cómo se guarda el dato internamente.
3. **Propiedades Calculadas:** Podemos crear un "getter" que combine varias propiedades (ej: `get nombreCompleto()`).

### Uso en el código
```javascript
let messi = new Futbolista("Lionel", "Messi", 800);

console.log(messi.goles); // Llama al get (Salida: 800)
messi.goles = 801;        // Llama al set
```

> **Nota:** Aunque usemos el guion bajo (`_`), en JavaScript estas propiedades siguen siendo accesibles técnicamente. Es una convención para indicar a otros programadores que no deben tocar esa variable directamente.

---
[[10-02-subclases|<- Anterior]] | [[00-indice-curso|Índice]] | [[10-04-veterinaria|Siguiente ->]]
