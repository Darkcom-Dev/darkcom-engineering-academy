# Lección 06: Bucle For-Of

El bucle `for-of` es una forma simplificada y moderna de recorrer **colecciones** (como Arrays o Strings) sin necesidad de manejar contadores o índices manualmente.

## Ventajas
- **Legibilidad:** El código es más limpio.
- **Seguridad:** Evita errores de "fuera de rango" en los índices.

### Ejemplo: Deletreando una palabra
```javascript
let palabra = "Hola";

// 'letra' tomará el valor de cada carácter en cada vuelta
for (let letra of palabra) {
    document.write(letra + "<br>");
}
```

### Recorriendo un Array
```javascript
let frutas = ["Manzana", "Pera", "Uva"];

for (let fruta of frutas) {
    console.log("Hoy comeré: " + fruta);
}
```

---
[[06-05-do-while|<- Anterior]] | [[00-indice-curso|Índice]] | [[06-07-break-continue|Siguiente: Break y Continue ->]]
