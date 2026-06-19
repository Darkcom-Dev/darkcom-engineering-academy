# Lección 07: Combinadores CSS

Los combinadores explican la relación entre dos o más selectores. Son fundamentales para estilar elementos basados en su posición respecto a otros.

## Tipos de Combinadores

### 1. Descendiente (Espacio)
Selecciona todos los elementos que están dentro de otro, sin importar el nivel de profundidad.
```css
div p { color: blue; } /* Párrafos dentro de un DIV */
```

### 2. Hijo Directo (`>`)
Selecciona solo los elementos que son hijos inmediatos.
```css
div > p { color: red; } /* Solo hijos directos */
```

### 3. Hermano Adyacente (`+`)
Selecciona el elemento que está inmediatamente después.
```css
h1 + p { font-weight: bold; } /* El primer párrafo después de un H1 */
```

### 4. Hermano General (`~`)
Selecciona todos los hermanos que vienen después.
```css
h1 ~ p { color: gray; } /* Todos los párrafos después de un H1 */
```

---
[[02-06-selectores|<- Anterior]] | [[00-indice-curso|Índice]] | [[02-08-biografia-v2|Siguiente: Proyecto Biografía V2 ->]]
