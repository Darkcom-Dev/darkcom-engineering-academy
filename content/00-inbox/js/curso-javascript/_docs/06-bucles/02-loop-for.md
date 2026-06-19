---
tema: "Bucles"
leccion: 02
titulo: "Loop For"
---

# Loop For

**Módulo:** `Bucles` | **Lección:** 02

## 🧩 Código JavaScript

```javascript
for(x=1; x<6; x++){
                document.write("<h" + x + ">Hola mundo</h" + x + ">");
            }
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
- [[Fizz Buzz]]
- [[Tablas de Multiplicar]]
- [[Loop Do... While]]
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
