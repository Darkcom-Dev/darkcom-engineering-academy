# Lección 06: Selectores CSS

Los selectores indican al navegador qué elementos HTML deben recibir un estilo determinado.

## Selectores Básicos

| Tipo | Selector | Ejemplo CSS | Descripción |
| --- | --- | --- | --- |
| **Universal** | `*` | `* { margin: 0; }` | Selecciona todos los elementos. |
| **Etiqueta** | `p`, `h1` | `p { color: gray; }` | Selecciona todos los párrafos. |
| **Clase** | `.nombre` | `.destacado { ... }` | Selecciona elementos con `class="nombre"`. |
| **ID** | `#id` | `#footer { ... }` | Selecciona el único elemento con `id="footer"`. |

### Selectores de Atributo
Puedes seleccionar elementos basados en sus atributos (como el destino de un enlace).

```css
/* Selecciona solo los enlaces que apuntan a una web específica */
a[href="https://google.com"] {
    color: green;
}
```

---
[[02-05-div-y-span|<- Anterior]] | [[00-indice-curso|Índice]] | [[02-07-combinadores|Siguiente: Combinadores CSS ->]]
