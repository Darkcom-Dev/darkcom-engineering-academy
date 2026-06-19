# Lección 04: Manejo de Errores Avanzado

El manejo de errores varía según el patrón de asincronía que estemos utilizando. Es vital saber cómo capturar excepciones para evitar que nuestra aplicación falle silenciosamente.

## 1. Errores en Callbacks
Por convención, en los callbacks se pasa el error como el **primer argumento**.

```javascript
function operacion(a, b, callback) {
    if (error) return callback(new Error("Fallo"));
    callback(null, "Exito");
}

operacion(1, 2, (err, res) => {
    if (err) return console.error(err);
    console.log(res);
});
```

## 2. Errores en Promesas
Usamos el método `.catch()` al final de la cadena de promesas.

```javascript
promesa()
    .then(res => ...)
    .catch(err => console.error("Capturado:", err));
```

## 3. Errores en Async/Await
Este es el método más limpio, ya que permite usar la estructura estándar `try...catch`.

```javascript
async function miFuncion() {
    try {
        let respuesta = await fetch(url);
        // Si el fetch falla o el código de abajo lanza un error...
        if (!respuesta.ok) throw new Error("Error de red");
    } catch (error) {
        // ...caemos directamente aquí
        console.error("Mensaje de error: " + error.message);
    }
}
```

## Resumen de Captura de Errores

| Técnica | Método de Captura |
| :--- | :--- |
| **Callbacks** | Primer argumento de la función (`err`). |
| **Promesas** | Bloque `.catch(err)`. |
| **Async/Await** | Bloque `try { ... } catch (err) { ... }`. |

```mermaid
flowchart TD
    A[Inicio Operación] --> B{¿Asíncrona?}
    B -- Callbacks --> C[Verificar primer param 'err']
    B -- Promesas --> D[Encadenar .catch()]
    B -- Async/Await --> E[Envolver en try/catch]
```

---
[[13-03-async-await|<- Anterior]] | [[00-indice-curso|Índice]] | [[13-05-cotizaciones|Siguiente ->]]
