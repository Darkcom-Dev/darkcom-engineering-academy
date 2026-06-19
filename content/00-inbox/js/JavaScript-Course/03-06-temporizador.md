# Lección 06: Temporizadores (`setTimeout`)

JavaScript nos permite programar tareas para que ocurran después de un tiempo determinado.

## La Función `setTimeout`
Esta función recibe dos parámetros:
1. **La función** que quieres ejecutar.
2. **El tiempo** de espera en **milisegundos** (1000ms = 1 segundo).

### Ejemplo: Una Alarma
```javascript
function comenzarTiempo(){
    let segundos = document.getElementById("tiempoElegido").value;
    // Programamos la alarma
    setTimeout(tiempoCumplido, 1000 * segundos);
}

function tiempoCumplido(){
    alert("¡Tiempo cumplido!");
}
```

## Flujo de Ejecución
```mermaid
sequenceDiagram
    participant U as Usuario
    participant JS as JavaScript
    participant B as Browser
    U->>JS: Clic en Alarma
    JS->>B: Programar setTimeout(10s)
    Note over B: Esperando 10 segundos...
    B->>JS: ¡Tiempo terminado!
    JS->>U: Muestra Alarma
```

---
[[03-05-mostrar-ingresos|<- Anterior]] | [[00-indice-curso|Índice]] | [[03-07-sonidos|Siguiente: Sonidos ->]]
