# Lección 07: Eventos Personalizados

Además de los eventos estándar (click, keydown), JavaScript permite crear nuestros propios eventos para comunicar diferentes partes de una aplicación de forma desacoplada.

## Creación y Disparo de Eventos
Usamos el constructor `CustomEvent` para definir el evento y el método `dispatchEvent` para lanzarlo sobre un elemento.

```javascript
// 1. Definir el evento
let miEvento = new CustomEvent('nombreDelEvento', {
    detail: { mensaje: "Hola Mundo" } // (Opcional) Pasar datos
});

// 2. Disparar el evento sobre un elemento
elemento.dispatchEvent(miEvento);
```

## Ejemplo Práctico: Reproductor de Música
Podemos lanzar un evento cada vez que la canción cambia para que otros componentes se actualicen.

```javascript
function cambiarCancion() {
    audio.src = nuevaRuta;
    audio.play();

    // Lanzamos el evento personalizado
    let evento = new CustomEvent('cambioDeCancion');
    audio.dispatchEvent(evento);
}

// Escuchamos el evento en cualquier otra parte del código
audio.addEventListener('cambioDeCancion', function() {
    console.log("Se ha cambiado la canción con éxito.");
});
```

## ¿Por qué usarlos?
- **Desacoplamiento:** La función que cambia la canción no necesita saber quién más está interesado en ese cambio.
- **Modularidad:** Puedes añadir múltiples "escuchadores" en diferentes archivos que reaccionen al mismo evento personalizado.

---
[[12-06-eventos-canvas|<- Anterior]] | [[00-indice-curso|Índice]] | [[12-08-buscador-peliculas|Siguiente ->]]
