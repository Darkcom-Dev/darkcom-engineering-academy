---
tema: "Bucles"
leccion: 05
titulo: "Titulo del Sitio"
---

# Titulo del Sitio

**Módulo:** `Bucles` | **Lección:** 05

## 🧩 Código JavaScript

```javascript
let nombre = "Fede";

            while (nombre != "Fede"){
                nombre = prompt("Dime mi nombre")
            }

            document.write("<h1>Hola " + nombre + "</h1>")
```


## 📊 Diagrama Conceptual

```mermaid
graph TD
    A[Bucles JS] --> B[for - clásico]
    A --> C[while]
    A --> D[do...while]
    A --> E[for...of]
    A --> F[for...in]
    B --> G[inicialización; condición; incremento]
    C --> H[Evalúa antes de iterar]
    D --> I[Ejecuta al menos una vez]
    E --> J[Valores de iterable]
    F --> K[Propiedades de objeto]
```

```mermaid
flowchart LR
    A[Sentencias] --> B[break]
    A --> C[continue]
    B --> D[Sale del bucle]
    C --> E[Salta a siguiente iteración]
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[Consola]]
- [[Loop For]]
- [[Fizz Buzz]]
- [[Tablas de Multiplicar]]
- [[Loop Do... While]]
- [[Loop For Of]]
- [[Break & Continue]]
- [[Etiquetas]]
- [[Boletín Estudiantil]]
- [[Boletin de Calificaciones]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Flujo del Programa]]
- ➡️ Módulo siguiente: [[Proyecto: Tienda de Donas]]
- 🏠 Volver al [[Índice del Curso]]
