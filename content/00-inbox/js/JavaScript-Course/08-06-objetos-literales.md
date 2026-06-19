# Lección 06: Otras Formas de Crear Objetos

Además de los objetos literales (`{}`) y las funciones constructoras, JavaScript ofrece otras maneras de instanciar objetos dependiendo de la necesidad.

## 1. Usando el Constructor Global `Object`
Podemos crear un objeto vacío usando `new Object()` y luego asignarle propiedades.

```javascript
let coche1 = new Object();

coche1.marca = "Chevrolet";
coche1["modelo"] = "Prisma";
coche1.encender = function() {
    console.log("Coche encendido");
};
```

También se le puede pasar un objeto inicial:
```javascript
let perro1 = new Object({ nombre: "Simba" });
```

## 2. Usando `Object.create()`
Este método crea un objeto nuevo utilizando un objeto existente como el **prototipo** del nuevo objeto.

```javascript
let coche2 = Object.create(coche1);
// coche2 hereda las propiedades y métodos de coche1
```

## Resumen de Métodos de Creación

| Método | Uso Principal | Ejemplo |
| :--- | :--- | :--- |
| **Literal** | Objetos simples y únicos | `{ nombre: "Simba" }` |
| **Constructor** | Múltiples objetos con el mismo molde | `new Perro("Raza", 5)` |
| **Object.create** | Herencia y prototipos | `Object.create(prototipo)` |

---
[[08-05-metodos-constructor|<- Anterior]] | [[00-indice-curso|Índice]] | [[08-07-for-in|Siguiente ->]]
