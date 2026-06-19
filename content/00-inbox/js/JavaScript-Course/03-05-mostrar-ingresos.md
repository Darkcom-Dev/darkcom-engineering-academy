# Lección 05: Modificar el Contenido (textContent)

En lugar de usar ventanas emergentes (`alert`), lo más común es mostrar la información directamente en la página modificando el DOM.

## La Propiedad `textContent`
Esta propiedad nos permite cambiar el texto que está dentro de una etiqueta HTML.

### Ejemplo Práctico
```html
<input type="text" id="nombreDeUsuario">
<button onclick="mostrarNombre()">Ingresar</button>
<h1 id="salida">No sé tu nombre</h1>

<script>
    function mostrarNombre(){
        let elementoNombre = document.getElementById("nombreDeUsuario");
        let elementoSalida = document.getElementById("salida");
        
        // Creamos el mensaje
        let mensaje = "Tú te llamas " + elementoNombre.value;
        
        // Modificamos el texto del H1 en tiempo real
        elementoSalida.textContent = mensaje;
    }
</script>
```

### Diferencia: .value vs .textContent
- **`.value`**: Se usa para etiquetas de entrada (`input`, `textarea`). Es lo que el usuario escribe.
- **`.textContent`**: Se usa para etiquetas de texto (`p`, `h1`, `span`, `div`). Es el texto que muestran.

---
[[03-04-tipos-de-datos|<- Anterior]] | [[00-indice-curso|Índice]] | [[03-06-temporizador|Siguiente: Temporizador ->]]
