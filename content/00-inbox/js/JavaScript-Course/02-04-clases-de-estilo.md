# Lección 04: Clases de Estilo

Las **clases** permiten aplicar estilos específicos a un grupo de elementos, independientemente de su etiqueta.

## ¿Cómo funcionan?
1. En el **HTML**, asignas una clase a un elemento usando el atributo `class`.
2. En el **CSS**, seleccionas esa clase usando un punto `.` antes del nombre.

### Ejemplo
**HTML:**
```html
<p class="destacado">Este párrafo es especial.</p>
<p class="normal">Este es un párrafo común.</p>
```

**CSS:**
```css
.destacado {
    color: orange;
    font-weight: bold;
}

.normal {
    color: gray;
}
```

### Diferencia clave: ID vs Clase
- **Clase (`.`):** Se puede usar en muchos elementos a la vez.
- **ID (`#`):** Es único. Solo debe haber un elemento con ese ID por página.

---
[[02-03-etiqueta-de-estilo|<- Anterior]] | [[00-indice-curso|Índice]] | [[02-05-div-y-span|Siguiente: DIV y SPAN ->]]
