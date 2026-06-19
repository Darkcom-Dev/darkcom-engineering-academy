---
tema: "Variables"
leccion: 05
titulo: "Ingresos de Usuario"
---

# Ingresos de Usuario

**Módulo:** `Variables` | **Lección:** 05

Ingresa tu nombre

🔹 **Campo** `text`: 

🔹 **Campo** `text`: Ingresar

# No sé tu nombre

## 🧩 Código JavaScript

```javascript
function mostrarNombre(){
                let elementoNombre = document.getElementById("nombreDeUsuario");
                let elementoTexto = document.getElementById("salida");
                let mensaje = "Tu te llamas " + elementoNombre.value
                


                elementoTexto.textContent = mensaje;
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
- [[Temporizador]]
- [[Sonidos]]
- [[Fecha y Hora]]
- [[Responde Rápido]]
- [[Concurso de preguntas]]

### Navegación del curso
- ⬅️ Módulo anterior: [[HTML Intermedio]]
- ➡️ Módulo siguiente: [[Funciones]]
- 🏠 Volver al [[Índice del Curso]]
