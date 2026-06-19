# Lección 02: Modificar Objetos

Los objetos en JavaScript son **dinámicos**. Esto significa que podemos cambiar sus valores, agregar nuevas propiedades o incluso eliminar propiedades existentes después de haber creado el objeto.

## Modificar Valores Existentes
Podemos reasignar el valor de una propiedad usando el signo de igual (`=`).

```javascript
let perro = {
    nombre: "Simba",
    edad: 4
};

// Modificamos la edad
perro.edad = 5; 
```

### Ejemplo Práctico: Función para incrementar valores
```javascript
function cumplirAnios() {
  perro.edad = perro.edad + 1;
}
```

## Agregar Nuevas Propiedades
Existen dos formas principales de agregar propiedades a un objeto que ya existe:

### 1. Notación de Punto
```javascript
perro.color = "Marrón";
```

### 2. Notación de Corchetes `[]`
Es útil cuando el nombre de la propiedad está en una variable o contiene caracteres especiales.
```javascript
perro["colorOjos"] = "Negro";
```

## Comparativa de Notaciones

| Característica | Notación de Punto (`.`) | Notación de Corchetes (`[]`) |
| :--- | :--- | :--- |
| **Sintaxis** | `objeto.propiedad` | `objeto["propiedad"]` |
| **Uso de Variables** | No permitido | Permitido: `objeto[variable]` |
| **Legibilidad** | Alta (Preferida) | Media |

---
[[08-01-objetos|<- Anterior]] | [[00-indice-curso|Índice]] | [[08-03-this|Siguiente ->]]
