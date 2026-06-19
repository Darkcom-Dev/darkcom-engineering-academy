---
tema: "Bucles"
leccion: 08
titulo: "Etiquetas"
---

# Etiquetas

**Módulo:** `Bucles` | **Lección:** 08

## 🧩 Código JavaScript

```javascript
let alto = 4;
            let ancho = 3;

            loopExterno:
            for (y = 1; y <= alto; y++) {
                for (x = 1; x <= ancho; x++) {
                    document.write(y + "." + x + "<br>");
                    if (y == 2 && x == 2) {
                        continue loopExterno;
                    }
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
- [[Loop For Of]]
- [[Break & Continue]]
- [[Boletín Estudiantil]]
- [[Boletin de Calificaciones]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Flujo del Programa]]
- ➡️ Módulo siguiente: [[Proyecto: Tienda de Donas]]
- 🏠 Volver al [[Índice del Curso]]
