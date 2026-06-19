---
tema: "Variables"
leccion: 06
titulo: "Temporizador"
---

# Temporizador

**Módulo:** `Variables` | **Lección:** 06

🔹 **Campo** `number`: 

🔹 **Campo** `text`: Alarma

# APAGADO

## 🧩 Código JavaScript

```javascript
let elementoSegundos = document.getElementById("tiempoElegido");
            let elementoTextoAlarma = document.getElementById("textoAlarma");

            function comenzarTiempo(){
                setTimeout(tiempoCumplido, 1000 * elementoSegundos.value);
            }

            function tiempoCumplido(){
                elementoTextoAlarma.textContent = "ENCENDIDO";
                elementoTextoAlarma.style.color = "green";
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
- [[Sonidos]]
- [[Fecha y Hora]]
- [[Responde Rápido]]
- [[Concurso de preguntas]]

### Navegación del curso
- ⬅️ Módulo anterior: [[HTML Intermedio]]
- ➡️ Módulo siguiente: [[Funciones]]
- 🏠 Volver al [[Índice del Curso]]
