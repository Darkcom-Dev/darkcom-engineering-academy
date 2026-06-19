# Lección 03: La Etiqueta de Estilo y Enlace CSS

Existen tres formas de aplicar CSS a una página, pero las más comunes son mediante la etiqueta `<style>` y archivos externos.

## 1. Estilos Internos (`<style>`)
Se definen dentro del `head` de la misma página HTML. Ideal para estilos rápidos o páginas únicas.

```html
<head>
    <style>
        h1 { color: blue; }
        p { font-family: Arial; }
    </style>
</head>
```

## 2. Archivos Externos (`<link>`)
Es la forma recomendada. Permite usar el mismo diseño en múltiples páginas.

```html
<head>
    <link rel="stylesheet" href="misEstilos.css">
</head>
```

### Ventajas de los Archivos Externos
- **Mantenibilidad:** Cambias un solo archivo y se actualizan todas tus páginas.
- **Limpieza:** El código HTML queda enfocado solo en el contenido.
- **Rendimiento:** El navegador puede cachear el archivo CSS.

---
[[02-02-dom|<- Anterior]] | [[00-indice-curso|Índice]] | [[02-04-clases-de-estilo|Siguiente: Clases de Estilo ->]]
