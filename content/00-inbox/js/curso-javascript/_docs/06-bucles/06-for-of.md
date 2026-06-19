---
tema: "Bucles"
leccion: 06
titulo: "Loop For Of"
---

# Loop For Of

**Módulo:** `Bucles` | **Lección:** 06

## 🧩 Código JavaScript

```javascript
let palabra = "Bienvenido";
            
            function loopear(){
                for(let letra of palabra) {
                document.write(letra + "<br>");
                }
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
- [[Loop For]]
- [[Fizz Buzz]]
- [[Tablas de Multiplicar]]
- [[Loop Do... While]]
- [[Titulo del Sitio]]
- [[Break & Continue]]
- [[Etiquetas]]
- [[Boletín Estudiantil]]
- [[Boletin de Calificaciones]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Flujo del Programa]]
- ➡️ Módulo siguiente: [[Proyecto: Tienda de Donas]]
- 🏠 Volver al [[Índice del Curso]]
