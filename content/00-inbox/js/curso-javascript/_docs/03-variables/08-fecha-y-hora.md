---
tema: "Variables"
leccion: 08
titulo: "Fecha y Hora"
---

# Fecha y Hora

**Módulo:** `Variables` | **Lección:** 08

🔹 **Campo** `number`: 

🔹 **Campo** `text`: Alarma

# 00:00:00

## 🧩 Código JavaScript

```javascript
let elementoSegundos = document.getElementById("tiempoElegido");
            let elementoTextoAlarma = document.getElementById("textoAlarma");
            let elementoSonidoAlarma = document.getElementById("audioAlarma");

            function comenzarTiempo(){
                setTimeout(tiempoCumplido, 1000 * elementoSegundos.value);
            }

            function tiempoCumplido(){
                elementoTextoAlarma.style.color = "green";
                elementoSonidoAlarma.play();
            }

            function comenzarReloj(){
                setInterval(ticTac, 1000);
            }

            function ticTac(){
                let tiempoActual = new Date();
                let hora = tiempoActual.getHours();
                let minutos = tiempoActual.getMinutes();
                let segundos = String(tiempoActual.getSeconds()).padStart(2, "0");

                let textoHora = hora + ":" + minutos + ":" + segundos;
                elementoTextoAlarma.textContent = textoHora;
            }
```


## 📊 Diagrama Conceptual

```mermaid
graph LR
    A[Variables JS] --> B[var - global/función]
    A --> C[let - bloque]
    A --> D[const - bloque, no reasignable]
    B --> E[Legacy]
    C --> F[Moderno]
    D --> G[Valores fijos]
```

```mermaid
graph TD
    A[Tipos de Datos JS] --> B[Primitivos]
    A --> C[Objetos]
    B --> D[string]
    B --> E[number]
    B --> F[boolean]
    B --> G[null]
    B --> H[undefined]
    B --> I[symbol]
    B --> J[bigint]
    C --> K[Object]
    C --> L[Array]
    C --> M[Function]
    C --> N[Date]
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[Saludo]]
- [[Ingresos de Usuario]]
- [[Variables]]
- [[Tipos de Datos]]
- [[Ingresos de Usuario]]
- [[Temporizador]]
- [[Sonidos]]
- [[Responde Rápido]]
- [[Concurso de preguntas]]

### Navegación del curso
- ⬅️ Módulo anterior: [[HTML Intermedio]]
- ➡️ Módulo siguiente: [[Funciones]]
- 🏠 Volver al [[Índice del Curso]]
