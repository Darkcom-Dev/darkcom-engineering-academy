# Lección 08: Fecha y Hora (`Date` y `setInterval`)

En esta lección combinamos el manejo del tiempo con el objeto `Date` para crear un reloj funcional.

## El Objeto `Date`
Proporciona métodos para obtener la fecha y hora actual del sistema.

```javascript
let ahora = new Date();
console.log(ahora.getHours());   // Hora (0-23)
console.log(ahora.getMinutes()); // Minutos (0-59)
console.log(ahora.getSeconds()); // Segundos (0-59)
```

## Ejecución Continua (`setInterval`)
A diferencia de `setTimeout`, que se ejecuta una sola vez, `setInterval` repite una función cada cierto tiempo de forma indefinida.

```javascript
// Ejecuta la función 'ticTac' cada 1 segundo (1000ms)
setInterval(ticTac, 1000);
```

### Formateo de Texto (`padStart`)
Para que los segundos siempre tengan dos dígitos (ej. `05` en lugar de `5`), usamos `padStart`.

```javascript
let seg = String(ahora.getSeconds()).padStart(2, "0");
```

---
[[03-07-sonidos|<- Anterior]] | [[00-indice-curso|Índice]] | [[03-09-proyecto-dia-3|Siguiente: Proyecto Día 3 ->]]
