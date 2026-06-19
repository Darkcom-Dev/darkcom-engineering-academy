---
modulo: "Eventos"
---

# Eventos

## 📊 Diagrama del Módulo

```mermaid
graph TD
    A[Evento] --> B[Tipo: click, keydown, etc]
    A --> C[Elemento objetivo]
    A --> D[Manejador - Handler]
    B --> E[Eventos de ratón]
    B --> F[Eventos de teclado]
    B --> G[Eventos de formulario]
    B --> H[Eventos personalizados]
    D --> I[addEventListener()]
    D --> J[onclick / onkeydown]
```

## Lecciones

- [[Introducción a los Eventos en JS]]
- [[Manejo de Eventos del DOM]]
- [[Eventos del teclado]]
- [[Eventos del Ratón]]
- [[Eventos en Tiempo Real]]
- [[Dibujando con el Mouse]]
- [[Dibujando con Flechas]]
- [[Eventos Personalizados]]
- [[Buscador]]
- [[Buscador de peliculas]]

---

⬅️ [[Manejo de Excepciones y JSON]] | [🏠 Índice del Curso](../index.md) | [[Promesas y Async/Await]] ➡️
