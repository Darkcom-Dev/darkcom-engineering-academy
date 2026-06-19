# Lección 02: Manejo de Eventos (addEventListener)

El método `addEventListener` es la forma estándar y recomendada de manejar eventos en JavaScript moderno. Permite añadir múltiples funciones a un mismo evento y ofrece un control mucho más preciso.

## Sintaxis
```javascript
elemento.addEventListener('tipoDeEvento', funcionCallback);
```

### Ejemplo Práctico
```javascript
let boton = document.getElementById('miBoton');

boton.addEventListener('click', function(event) {
    console.log("Evento disparado en: " + event.target);
});
```

## Conceptos Clave del Objeto Event

### 1. `event.target` vs `event.currentTarget`
- **`target`**: Es el elemento exacto que disparó el evento (donde hiciste click).
- **`currentTarget`**: Es el elemento que tiene el "escuchador" de eventos.

### 2. `event.preventDefault()`
Este método es vital para cancelar el comportamiento por defecto del navegador. Por ejemplo:
- Evitar que un enlace abra una página.
- Evitar que un formulario se envíe y recargue la página.

```javascript
enlace.addEventListener('click', function(event) {
    event.preventDefault(); // El enlace no navegará
    alert('Navegación cancelada');
});
```

## ¿Por qué usar addEventListener?
1. **Múltiples escuchadores:** Puedes tener 5 funciones diferentes que se activen al hacer click en el mismo botón.
2. **Separación de capas:** Mantiene el HTML limpio de código JavaScript.
3. **Control total:** Puedes remover el escuchador en cualquier momento con `removeEventListener`.

---
[[12-01-eventos|<- Anterior]] | [[00-indice-curso|Índice]] | [[12-03-eventos-teclado|Siguiente ->]]
