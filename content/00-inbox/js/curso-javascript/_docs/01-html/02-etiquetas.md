---
tema: "HTML Básico"
leccion: 02
titulo: "Mis etiquetas"
---

# Mis etiquetas

**Módulo:** `HTML Básico` | **Lección:** 02

Este es el esquema mínimo para un página web

El html es el esquema del documento

Head es el encabezado de página

Para ver como se esccriben las etiquetas, ve al inspector de elementos


## 📊 Diagrama Conceptual

```mermaid
graph TD
    A[HTML Document] --> B[&lt;head&gt;]
    A --> C[&lt;body&gt;]
    B --> D[&lt;title&gt;]
    B --> E[&lt;style&gt;]
    B --> F[&lt;link&gt;]
    B --> G[&lt;meta&gt;]
    C --> H[&lt;h1&gt;-&lt;h6&gt;]
    C --> I[&lt;p&gt;]
    C --> J[&lt;a&gt;]
    C --> K[&lt;img&gt;]
    C --> L[&lt;ul&gt;/&lt;ol&gt;]
    C --> M[&lt;table&gt;]
    C --> N[&lt;div&gt;/&lt;span&gt;]
    C --> O[&lt;script&gt;]
```

```mermaid
graph LR
    A[Texto] --> B[&lt;em&gt; - Énfasis]
    A --> C[&lt;i&gt; - Cursiva]
    A --> D[&lt;strong&gt; - Importante]
    A --> E[&lt;b&gt; - Negrita]
    A --> F[&lt;small&gt; - Pequeño]
    A --> G[&lt;del&gt; - Tachado]
    A --> H[&lt;ins&gt; - Insertado]
    A --> I[&lt;u&gt; - Subrayado]
    A --> J[&lt;mark&gt; - Marcado]
```

```mermaid
graph LR
    A[Listas HTML] --> B[&lt;ol&gt; - Lista Ordenada]
    A --> C[&lt;ul&gt; - Lista No Ordenada]
    B --> D[&lt;li&gt;]
    C --> D
```

```mermaid
graph LR
    A[&lt;table&gt;] --> B[&lt;tr&gt; - Fila]
    B --> C[&lt;td&gt; - Celda]
    B --> D[&lt;th&gt; - Encabezado]
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[Mi Primera Página]]
- [[Etiquetas anidadas]]
- [[Texto ordenado]]
- [[Texto ordenado]]
- [[Agregar Imágenes]]
- [[Enlaces]]
- [[Mi Biografia]]
- [[Pagina 2]]
- [[Mi Biografia]]
- [[Mi Biografia]]
- [[Mi Biografia]]

### Navegación del curso
- ➡️ Módulo siguiente: [[HTML Intermedio]]
- 🏠 Volver al [[Índice del Curso]]
