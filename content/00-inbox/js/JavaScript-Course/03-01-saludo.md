# Lección 01: Tu primer Script (Saludo)

JavaScript es el lenguaje que da vida a las páginas web. En esta lección, aprenderemos a crear una interacción simple.

## La Función `alert()`
La función `alert()` muestra una ventana emergente (pop-up) con un mensaje para el usuario. Es la forma más básica de mostrar información.

### Ejemplo de Código
```html
<button onclick="saludar()">Saludar</button>

<script>
    // Definimos la función saludar
    function saludar(){
        alert("Hola Mundo!");
    }
</script>
```

## Conceptos Clave
- **Evento `onclick`:** Es un atributo que le dice al navegador: "Cuando el usuario haga clic aquí, ejecuta este código".
- **Etiqueta `<script>`:** Aquí es donde vive nuestro código JavaScript.

---
[[02-08-biografia-v2|<- Anterior]] | [[00-indice-curso|Índice]] | [[03-02-ingreso-de-usuario|Siguiente: Ingreso de Usuario ->]]
