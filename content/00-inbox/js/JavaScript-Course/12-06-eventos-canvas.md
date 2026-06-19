# Lección 06: Eventos en Canvas

El elemento **Canvas** permite dibujar gráficos 2D mediante scripts. Al combinarlo con eventos, podemos crear aplicaciones interactivas como pizarras de dibujo o videojuegos simples.

## Interacción con el Ratón
Para crear una pizarra, necesitamos detectar cuándo el usuario está presionando el botón del ratón y cuándo lo está moviendo.

### Lógica de Dibujo
1. **`mousedown`:** El usuario empieza a dibujar (activamos una bandera `drawing = true`).
2. **`mousemove`:** Si la bandera es verdadera, trazamos líneas entre la posición anterior y la actual.
3. **`mouseup`:** El usuario deja de dibujar (desactivamos la bandera).

```javascript
let drawing = false;

canvas.addEventListener('mousedown', () => drawing = true);
canvas.addEventListener('mouseup', () => drawing = false);

canvas.addEventListener('mousemove', (event) => {
    if (drawing) {
        drawLine(event.layerX, event.layerY);
    }
});
```

## Interacción con el Teclado
Podemos usar las flechas del teclado para mover un "puntero" y dibujar líneas paso a paso.

```javascript
document.addEventListener('keydown', (event) => {
    switch(event.keyCode) {
        case 38: // Flecha Arriba
            drawLine(x, y, x, y - 10);
            y -= 10;
            break;
        // ... otros casos
    }
});
```

## Métodos Clave de Canvas
- **`getContext('2d')`:** Obtiene el lienzo para dibujar.
- **`beginPath()` / `closePath()`:** Inicia y termina un trazo.
- **`moveTo(x, y)`:** Mueve el "lápiz" a una posición sin dibujar.
- **`lineTo(x, y)`:** Dibuja una línea desde la posición actual a la nueva.
- **`stroke()`:** Aplica el color y dibuja efectivamente el trazo.
- **`clearRect(0, 0, w, h)`:** Borra una sección (o todo) el lienzo.

---
[[12-05-eventos-tiempo-real|<- Anterior]] | [[00-indice-curso|Índice]] | [[12-07-eventos-personalizados|Siguiente ->]]
