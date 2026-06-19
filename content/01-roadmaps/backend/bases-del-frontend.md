# Bases del Frontend

El **frontend** es la parte visible de una aplicación web: todo lo que el usuario ve, toca y con lo que interactúa en el navegador. Se compone de tres pilares fundamentales que trabajan en equipo.

```mermaid
graph TB
    Web["🌐 Aplicación Web"] --> HTML["📄 HTML<br/>Estructura"]
    Web --> CSS["🎨 CSS<br/>Estilos"]
    Web --> JS["⚡ JavaScript<br/>Comportamiento"]

    subgraph Analogia ["Analogía: Una Casa"]
        H["🏠 HTML = Esqueleto<br/>(paredes, puertas, cuartos)"]
        C["🖌️ CSS = Decoración<br/>(colores, muebles, jardín)"]
        J["🔌 JS = Electricidad<br/>(luces, timbre, automatización)"]
    end

    style HTML fill:#e65100,color:#fff
    style CSS fill:#1565c0,color:#fff
    style JS fill:#fdd835,color:#333
    style Analogia fill:#f5f5f5
```

---

## HTML

**HTML** (HyperText Markup Language) es el lenguaje de marcado que define la **estructura y el contenido** de una página web. No es un lenguaje de programación, sino de **marcado**: usa etiquetas (tags) para decirle al navegador qué es cada cosa.

### Anatomía de una etiqueta HTML

```mermaid
graph LR
    subgraph Etiqueta ["<img src='foto.jpg' alt='Texto' />"]
        Apertura["<img"] --> Atributos["src='foto.jpg'<br/>alt='Texto'"]
        Atributos --> Cierre["/>"]
    end

    subgraph Explicacion ["📖 Lectura"]
        L1["img → elemento: imagen"]
        L2["src → atributo: ruta del archivo"]
        L3["alt → atributo: texto alternativo"]
    end

    style Etiqueta fill:#fff3e0
    style Explicacion fill:#e8f5e9
```

### Estructura básica de un documento HTML

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Mi página</title>
</head>
<body>
    <header>
        <h1>Título principal</h1>
        <nav>Menú de navegación</nav>
    </header>
    <main>
        <article>
            <h2>Artículo</h2>
            <p>Contenido del artículo</p>
        </article>
    </main>
    <footer>Pie de página</footer>
</body>
</html>
```

```mermaid
flowchart TB
    subgraph HTML ["📄 Documento HTML"]
        DOCTYPE["<!DOCTYPE html>"] --> Html["<html>"]
        Html --> Head["<head>"]
        Html --> Body["<body>"]

        Head --> Meta["<meta charset='UTF-8'>"]
        Head --> Title["<title>"]

        Body --> Header["<header>"]
        Body --> Main["<main>"]
        Body --> Footer["<footer>"]

        Header --> H1["<h1> Título"]
        Header --> Nav["<nav> Menú"]

        Main --> Article["<article>"]
        Article --> H2["<h2> Subtítulo"]
        Article --> P["<p> Párrafo"]
        Article --> Img["<img>"]
    end

    style HTML fill:#fff3e0
    style DOCTYPE fill:#ffe0b2
    style Head fill:#ffccbc
    style Body fill:#ffccbc
```

### Etiquetas HTML más comunes

| Categoría       | Etiquetas                            | Función                        |
| --------------- | ------------------------------------ | ------------------------------ |
| **Texto**       | `<h1>`-`<h6>`, `<p>`, `<span>`      | Títulos y párrafos             |
| **Enlaces**     | `<a>`                                | Hipervínculos                  |
| **Multimedia**  | `<img>`, `<video>`, `<audio>`        | Imágenes, videos, audio        |
| **Listas**      | `<ul>`, `<ol>`, `<li>`               | Listas ordenadas y no ordenadas |
| **Tablas**      | `<table>`, `<tr>`, `<td>`            | Datos tabulares                |
| **Formularios** | `<form>`, `<input>`, `<button>`      | Entrada de datos del usuario   |
| **Semánticos**  | `<header>`, `<nav>`, `<main>`, `<footer>`, `<article>`, `<section>` | Dan significado estructural |

```mermaid
graph LR
    subgraph Semantica ["🎯 HTML Semántico"]
        Antes["<div class='header'>"] -->|"❌ Sin significado"| Nada
        Despues["<header>"] -->|"✅ Describe su propósito"| Accesibilidad["♿ Mejor accesibilidad<br/>🔍 Mejor SEO<br/>🧹 Código más limpio"]
    end

    style Semantica fill:#e8f5e9
    style Antes fill:#ffcdd2
    style Despues fill:#c8e6c9
```

> **Regla de oro:** El HTML debe describir **qué es el contenido**, no cómo se ve. Para eso está CSS.

---

## CSS

**CSS** (Cascading Style Sheets) es el lenguaje que controla la **presentación visual** de los elementos HTML. Colores, fuentes, tamaños, posiciones, animaciones... todo eso es responsabilidad de CSS.

### Anatomía de una regla CSS

```css
selector {
    propiedad: valor;
}
```

```mermaid
graph LR
    subgraph Regla ["p { color: blue; font-size: 16px; }"]
        Selector["p"] --> Declaraciones["{ ... }"]
        Declaraciones --> Prop1["color: blue;"]
        Declaraciones --> Prop2["font-size: 16px;"]
    end

    subgraph Lectura ["📖 Se lee:"]
        L1["A todos los &lt;p&gt;"]
        L2["ponles color azul"]
        L3["y tamaño de fuente 16px"]
    end

    style Regla fill:#e3f2fd
    style Lectura fill:#e8f5e9
```

### Formas de aplicar CSS

```mermaid
flowchart TB
    A["🎨 CSS"] --> Inline["✏️ En línea<br/>style='color:red'"]
    A --> Interno["📄 Interno<br/>&lt;style&gt; en &lt;head&gt;"]
    A --> Externo["📁 Externo<br/>archivo .css separado"]

    Inline -->|"⚠️ Prioridad alta<br/>pero difícil de mantener"| Preferencia
    Interno -->|"⚠️ Útil para pages<br/>únicas"| Preferencia
    Externo -->|"✅ Recomendado<br/>reutilizable y modular"| Preferencia

    style Inline fill:#ffccbc
    style Interno fill:#ffe0b2
    style Externo fill:#c8e6c9
    style Preferencia fill:#e8f5e9
```

### Selectores CSS

```css
/* Selector de etiqueta */
p     { color: red; }

/* Selector de clase */
.card { background: white; }

/* Selector de ID */
#logo { width: 100px; }

/* Selector anidado */
div p { margin: 0; }       /* <p> dentro de <div> */

/* Selector múltiple */
h1, h2 { font-weight: bold; }

/* Pseudo-clases */
a:hover { color: green; }  /* cuando pasa el mouse */
```

```mermaid
graph TB
    subgraph Especificidad ["⚖️ Cascada y Especificidad"]
        Universal["* {}<br/>Selector universal"]:::baja
        Etiqueta["p, h1 {}<br/>Selectores de etiqueta"]:::baja
        Clase[".card, [attr] {}<br/>Clases y atributos"]:::media
        ID["#header {}<br/>ID"]:::alta
        Inline["style=''<br/>Estilo en línea"]:::muyalta
        Important["!important<br/>⚠️ Rompe la cascada"]:::rompe
    end

    subgraph Ejemplo ["🇪🇦 Si hay conflicto:"]
        A["p { color: red; }"]
        B[".texto { color: blue; }"]
        C["#parrafo { color: green; }"]
        A --> R1["❌ Pierde"]
        B --> R1
        C --> R1
        R1 --> Gana["✅ Gana #parrafo"]
    end

    classDef baja fill:#e8f5e9
    classDef media fill:#fff9c4
    classDef alta fill:#ffe0b2
    classDef muyalta fill:#ffccbc
    classDef rompe fill:#ffcdd2
```

### El Modelo de Caja (Box Model)

Todo elemento HTML es una **caja rectangular**. CSS te permite controlar cada capa de esa caja.

```mermaid
flowchart TB
    subgraph Box ["📦 Box Model"]
        Margin["🔲 Margin<br/>(Margen exterior - transparente)"]
        Border["🔷 Border<br/>(Borde)"]
        Padding["🔶 Padding<br/>(Relleno interior)"]
        Content["⬜ Content<br/>(Contenido: texto, imagen)"]
    end

    Content --> Padding
    Padding --> Border
    Border --> Margin

    style Content fill:#bbdefb
    style Padding fill:#ffe0b2
    style Border fill:#c8e6c9
    style Margin fill:#f5f5f5,stroke-dasharray: 5 5
```

```
                         ╔══════════════════════╗
                         ║      MARGIN          ║
                         ║   ┌──────────────┐   ║
                         ║   ║    BORDER     ║   ║
                         ║   ║  ┌────────┐   ║   ║
                         ║   ║  ║ PADDING ║   ║   ║
                         ║   ║  ║ ┌──────┐║   ║   ║
                         ║   ║  ║ │CONTENT│║   ║   ║
                         ║   ║  ║ └──────┘║   ║   ║
                         ║   ║  └────────┘║   ║   ║
                         ║   ╚════════════╝   ║   ║
                         ╚══════════════════════╝
```

### Layout: cómo posicionar elementos

```mermaid
flowchart TB
    Layout["📐 Sistemas de Layout"] --> Normal["Normal Flow<br/>(por defecto)"]
    Layout --> Flexbox["📏 Flexbox<br/>(1 dimensión)"]
    Layout --> Grid["📐 CSS Grid<br/>(2 dimensiones)"]
    Layout --> Position["📍 Position<br/>(posicionamiento)"]

    Flexbox --> F1["✅ Ideal para barras<br/>de navegación"]
    Flexbox --> F2["✅ Centrar elementos"]
    Flexbox --> F3["✅ Distribuir espacio"]

    Grid --> G1["✅ Layouts completos"]
    Grid --> G2["✅ Galerías de imágenes"]
    Grid --> G3["✅ Dashboards complejos"]

    Position --> P1["static (defecto)"]
    Position --> P2["relative (relativo)"]
    Position --> P3["absolute (absoluto)"]
    Position --> P4["fixed (fijo en pantalla)"]
    Position --> P5["sticky (pegajoso)"]

    style Layout fill:#e3f2fd
    style Flexbox fill:#e8f5e9
    style Grid fill:#fff3e0
    style Position fill:#fce4ec
```

> **💡 Dato clave:** Flexbox es unidimensional (fila O columna). Grid es bidimensional (filas Y columnas). Se complementan, no compiten.

### Unidades CSS

| Unidad  | Relativa a              | Ejemplo              |
| ------- | ----------------------- | -------------------- |
| `px`    | Píxel físico            | `width: 300px`       |
| `%`     | Elemento padre          | `width: 50%`         |
| `em`    | Tamaño de fuente del padre | `padding: 2em`    |
| `rem`   | Tamaño de fuente raíz   | `font-size: 1.5rem`  |
| `vw`    | 1% del ancho del viewport | `width: 100vw`    |
| `vh`    | 1% del alto del viewport | `height: 100vh`   |
| `ch`    | Ancho del carácter `0`  | `max-width: 60ch`    |

> **💡 Recomendación:** Usa `rem` para fuentes (accesibilidad), `%`/`vw`/`vh` para layouts responsive, y `px` solo para bordes finos o elementos fijos pequeños.

---

## JavaScript

**JavaScript** es el lenguaje de programación del frontend. Hace que las páginas web **reaccionen** a las acciones del usuario: clics, formularios, animaciones, peticiones al servidor, etc.

```mermaid
graph TB
    subgraph QueHace ["🧠 ¿Qué hace JS?"]
        Manipular["🔄 Manipular el DOM<br/>(añadir/eliminar HTML"]
        Eventos["👆 Responder a eventos<br/>(click, teclear, mover mouse)"]
        Comunicacion["📡 Comunicarse con servidores<br/>(fetch / AJAX)"]
        Animaciones["✨ Crear animaciones<br/>y transiciones"]
        Logica["🧮 Lógica de negocio<br/>(validaciones, cálculos)"]
    end

    JS["⚡ JavaScript"] --> QueHace

    style JS fill:#fdd835,color:#333
    style QueHace fill:#fff8e1
```

### Conceptos fundamentales

```mermaid
flowchart LR
    subgraph Fundamentos ["🔤 Fundamentos de JS"]
        Vars["Variables<br/>let, const, var"]
        Types["Tipos de datos<br/>string, number, boolean,<br/>array, object"]
        Funcs["Funciones<br/>function, arrow =>"]
        Control["Control de flujo<br/>if, for, while"]
        DOM["DOM<br/>document.querySelector()"]
        Events["Eventos<br/>addEventListener()"]
    end

    Vars --> Types --> Funcs --> Control --> DOM --> Events

    style Fundamentos fill:#fff8e1
```

### El DOM (Document Object Model)

Cuando el navegador lee HTML, crea un **árbol de nodos** en memoria llamado DOM. JavaScript puede navegar y modificar ese árbol.

```mermaid
graph TB
    subgraph DOMTree ["🌳 Árbol del DOM"]
        Document["document"] --> Html["html"]
        Html --> Head["head"]
        Html --> Body["body"]

        Head --> Meta["meta"]
        Head --> Title["title"]

        Body --> Header["header"]
        Body --> Main["main"]
        Body --> Footer["footer"]

        Header --> H1["h1"]
        Header --> Nav["nav"]

        Main --> Article["article"]
        Article --> H2["h2"]
        Article --> P["p"]
        Article --> Button["button"]
    end

    JS["⚡ JavaScript"] -.-> |"document.querySelector('h1')<br/>h1.textContent = 'Nuevo título'"| H1
    JS -.-> |"button.addEventListener('click', ...)"| Button

    style DOMTree fill:#e8f5e9
    style JS fill:#fdd835,color:#333
```

### Ejemplos básicos

```javascript
// Variables
let nombre = "Ana";
const edad = 25;
var antigua = "No usar";  // ❌ obsoleto

// Tipos de datos
let texto = "Hola";          // string
let numero = 42;             // number
let activo = true;           // boolean
let colores = ["rojo", "verde", "azul"]; // array
let persona = {              // object
    nombre: "Ana",
    edad: 25
};

// Funciones
function saludar(nombre) {
    return `Hola, ${nombre}!`;
}

const sumar = (a, b) => a + b;  // arrow function

// Manipular el DOM
const titulo = document.querySelector("h1");
titulo.textContent = "Nuevo título";
titulo.style.color = "blue";

// Eventos
const boton = document.querySelector("button");
boton.addEventListener("click", () => {
    alert("¡Click!");
});

// Petición al servidor (fetch)
fetch("https://api.ejemplo.com/datos")
    .then(respuesta => respuesta.json())
    .then(datos => console.log(datos));
```

### Eventos del navegador

```mermaid
flowchart TB
    subgraph Eventos ["👆 Eventos comunes"]
        Mouse["🖱️ Mouse<br/>click, dblclick,<br/>mouseover, mouseout"]
        Teclado["⌨️ Teclado<br/>keydown, keyup,<br/>keypress"]
        Formulario["📝 Formulario<br/>submit, change,<br/>focus, blur"]
        Ventana["🪟 Ventana<br/>scroll, resize,<br/>load, DOMContentLoaded"]
    end

    Element["Elemento HTML"] --> addEventListener["element.addEventListener('click', handler)"]
    addEventListener --> Callback["📞 Función callback<br/>se ejecuta cuando ocurre el evento"]
    Callback --> EventObj["📦 Event Object<br/>event.target, event.type"]

    style Eventos fill:#fce4ec
    style Element fill:#e1f5fe
```

### Flujo de eventos: Burbujeo vs Captura

```mermaid
flowchart TD
    subgraph Captura ["⬇️ Fase de Captura"]
        Doc1["document"] --> Body1["body"]
        Body1 --> Div1["div"]
        Div1 --> Button1["button 🎯"]
    end

    subgraph Burbujeo ["⬆️ Fase de Burbujeo"]
        Button2["button 🎯"] --> Div2["div"]
        Div2 --> Body2["body"]
        Body2 --> Doc2["document"]
    end

    style Captura fill:#e3f2fd
    style Burbujeo fill:#fff3e0
    style Button1 fill:#ffcdd2
    style Button2 fill:#c8e6c9
```

> 💡 Por defecto, los eventos **burbujean** (van del elemento más específico al más general). Puedes detenerlo con `event.stopPropagation()`.

### Cómo se integran HTML, CSS y JS

```mermaid
sequenceDiagram
    participant HTML as 📄 HTML
    participant CSS as 🎨 CSS
    participant JS as ⚡ JavaScript
    participant Browser as 🌍 Navegador

    Note over HTML,Browser: El navegador recibe el archivo HTML

    Browser->>HTML: ① Parsear HTML
    HTML->>Browser: ② Crear DOM Tree

    alt Tiene CSS
        Browser->>CSS: ③ Parsear CSS
        CSS->>Browser: ④ Crear CSSOM
    end

    alt Tiene JS
        Browser->>JS: ⑤ Ejecutar JS
        JS->>HTML: ⑥ Puede modificar el DOM
        JS->>CSS: ⑦ Puede modificar estilos
    end

    Browser->>Browser: ⑧ Construir Render Tree
    Browser->>Browser: ⑨ Pintar en pantalla 🎉

    Note over HTML, Browser: Resultado final: página interactiva
```

---

## Resumen visual: el ecosistema frontend

```mermaid
graph TB
    Dev["👨‍💻 Desarrollador"] --> HTML["📄 HTML<br/>👉 Contenido"]
    Dev --> CSS["🎨 CSS<br/>👉 Presentación"]
    Dev --> JS["⚡ JS<br/>👉 Comportamiento"]

    HTML --> Navegador["🌍 Navegador"]
    CSS --> Navegador
    JS --> Navegador

    Navegador --> Usuario["👤 Usuario final<br/>ve la página"]

    subgraph Tools ["🛠️ Herramientas modernas (opcionales)"]
        Prepro["Preprocesadores<br/>SASS, LESS"]
        Framework["Frameworks CSS<br/>Bootstrap, Tailwind"]
        Lib["Librerías JS<br/>React, Vue, Svelte"]
        Bundler["Bundlers<br/>Vite, Webpack"]
    end

    CSS -.-> Prepro
    CSS -.-> Framework
    JS -.-> Lib
    JS -.-> Bundler

    style Dev fill:#e1f5fe
    style HTML fill:#e65100,color:#fff
    style CSS fill:#1565c0,color:#fff
    style JS fill:#fdd835,color:#333
    style Navegador fill:#f3e5f5
    style Usuario fill:#e8f5e9
    style Tools fill:#f5f5f5
```

### Los tres pilares resumidos

| Pilar       | Función           | Analogía           | Sintaxis           |
| ----------- | ----------------- | ------------------ | ------------------ |
| **HTML**    | Estructura        | 🏠 Esqueleto       | `<etiqueta>`      |
| **CSS**     | Estilo            | 🖌️ Decoración      | `selector { ... }` |
| **JS**      | Interactividad    | 🔌 Electricidad    | `código lógico`    |

> **Siguiente paso:** Profundiza en cada tecnología por separado y luego elige un framework como React, Vue o Svelte para construir aplicaciones más complejas.

## Relacionados:
- [[backend-introduccion]] #anterior  
- [[elige-un-lenguaje-de-backend]] #siguiente 