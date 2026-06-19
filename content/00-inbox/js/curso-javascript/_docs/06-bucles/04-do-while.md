---
tema: "Bucles"
leccion: 04
titulo: "Loop Do... While"
---

# Loop Do... While

**Módulo:** `Bucles` | **Lección:** 04

## 🧩 Código JavaScript

```javascript
let nombre;

            do {
                nombre = prompt("Dime mi nombre");
            } while (nombre != "Fede");

            document.write("<h1>Ese es mi nombre!</h1>")
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
- [[Titulo del Sitio]]
- [[Loop For Of]]
- [[Break & Continue]]
- [[Etiquetas]]
- [[Boletín Estudiantil]]
- [[Boletin de Calificaciones]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Flujo del Programa]]
- ➡️ Módulo siguiente: [[Proyecto: Tienda de Donas]]
- 🏠 Volver al [[Índice del Curso]]
