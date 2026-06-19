# Lección 02: Etiquetas de Texto

HTML ofrece diversas etiquetas para dar formato y significado al texto. Es importante distinguir entre etiquetas puramente visuales y etiquetas con significado semántico.

## Formatos Comunes

| Etiqueta | Función | Ejemplo |
| --- | --- | --- |
| `<em>` | Énfasis (itálica semántica) | *Énfasis* |
| `<i>` | Itálica (visual) | _Itálica_ |
| `<strong>` | Importancia fuerte (negrita semántica) | **Fuerte** |
| `<b>` | Negrita (visual) | **Negrita** |
| `<small>` | Texto pequeño | <small>Pequeño</small> |
| `<del>` | Texto tachado (eliminado) | ~~Tachado~~ |
| `<ins>` | Texto insertado (subrayado) | <u>Insertado</u> |
| `<mark>` | Texto resaltado (marcador) | ==Marcado== |

### Ejemplo en Código
```html
<body>
    Esta <em>palabra</em> tiene énfasis<br>
    Esta <strong>palabra</strong> es muy fuerte<br>
    Esta <mark>palabra</mark> está marcada<br>
</body>
```

> [!TIP]
> Hoy en día se prefiere el uso de etiquetas semánticas (`<em>`, `<strong>`) sobre las puramente visuales (`<i>`, `<b>`) para mejorar la accesibilidad y el SEO.

---
[[01-01-mi-primera-pagina|<- Anterior]] | [[00-indice-curso|Índice]] | [[01-03-etiquetas-anidadas|Siguiente: Etiquetas Anidadas ->]]
