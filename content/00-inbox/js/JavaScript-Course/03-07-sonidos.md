# Lección 07: Multimedia (Sonidos)

Podemos controlar elementos multimedia como audio y video directamente desde JavaScript.

## La Etiqueta `<audio>`
Se utiliza para incrustar archivos de sonido. Podemos ocultar los controles y manejarlos mediante código.

### Métodos de Audio
- `.play()`: Inicia la reproducción.
- `.pause()`: Pausa la reproducción.
- `.load()`: Recarga el archivo.

### Ejemplo: Alarma con Sonido
```html
<audio id="audioAlarma">
    <source src="static/media/sonido.mp3" type="audio/mpeg">
</audio>

<script>
    let sonido = document.getElementById("audioAlarma");

    function tiempoCumplido(){
        // Reproducimos el sonido cuando el tiempo acaba
        sonido.play();
    }
</script>
```

---
[[03-06-temporizador|<- Anterior]] | [[00-indice-curso|Índice]] | [[03-08-fecha-y-hora|Siguiente: Fecha y Hora ->]]
