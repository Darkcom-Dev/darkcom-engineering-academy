# Lección 01: La Consola del Desarrollador

Antes de sumergirnos en los bucles, debemos conocer nuestra mejor herramienta de depuración: la **Consola**.

## ¿Qué es?
Es una pestaña dentro de las herramientas de desarrollador del navegador (F12) que permite ver mensajes, errores y resultados de nuestro código sin interrumpir la experiencia del usuario con `alert()`.

### El método `console.log()`
Es la forma de enviar información a la consola.

```javascript
function calcular() {
    let resultado = 9 * 10;
    // Esto se verá solo en la consola
    console.log("El resultado es: " + resultado);
}
```

## Beneficios
1. **No bloquea:** A diferencia de `alert`, no detiene la ejecución del programa.
2. **Historial:** Puedes ver muchos mensajes a la vez.
3. **Inspección:** Permite ver el contenido completo de objetos y arrays de forma estructurada.

---
[[05-05-proyecto-flujo|<- Anterior]] | [[00-indice-curso|Índice]] | [[06-02-loop-for|Siguiente: Bucle For ->]]
