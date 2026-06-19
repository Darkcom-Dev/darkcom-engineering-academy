# Lección 06: Imágenes

Las imágenes se insertan con la etiqueta `<img>`, que es una etiqueta de auto-cierre (no necesita `</img>`).

## Atributos Principales
- `src`: La ruta (URL o archivo local) de la imagen.
- `alt`: Texto alternativo si la imagen no carga (fundamental para lectores de pantalla).
- `width` / `height`: Dimensiones de la imagen.

### Ejemplo: El Tucán
```html
<img src="static/img/tucan.jpg"
     width="50%"
     alt="Un hermoso tucán"
     align="left"
     hspace="20">
```

### Anatomía de la etiqueta <img>
```mermaid
graph LR
    IMG[Etiqueta img] --> SRC[src: Origen]
    IMG --> ALT[alt: Descripción]
    IMG --> DIM[width/height: Tamaño]
    IMG --> STY[align/hspace: Estilo antiguo]
```

---
[[01-05-encabezados|<- Anterior]] | [[00-indice-curso|Índice]] | [[01-07-enlaces|Siguiente: Enlaces ->]]
