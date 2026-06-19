---
title: "Minicurso de Programación Básica - JavaScript"
---

# 🎓 Minicurso de Programación Básica

> *"Esta web es un minicurso de programación básica, principalmente JavaScript"*

Bienvenido al minicurso. Aquí encontrarás lecciones organizadas por módulos que cubren desde HTML básico hasta conceptos avanzados de JavaScript.

---

## 📚 Módulos del Curso

### HTML Básico
**Lecciones:** 12

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
📖 [[HTML Básico|Ir al módulo]]

---

### HTML Intermedio
**Lecciones:** 13

```mermaid
graph TD
    A[CSS en HTML] --> B[&lt;style&gt; - Interno]
    A --> C[&lt;link&gt; - Archivo externo]
    A --> D[style='' - En línea]
```
📖 [[HTML Intermedio|Ir al módulo]]

---

### Variables
**Lecciones:** 10

```mermaid
graph LR
    A[Variables JS] --> B[var - global/función]
    A --> C[let - bloque]
    A --> D[const - bloque, no reasignable]
    B --> E[Legacy]
    C --> F[Moderno]
    D --> G[Valores fijos]
```
📖 [[Variables|Ir al módulo]]

---

### Funciones
**Lecciones:** 9

```mermaid
flowchart TD
    A[Función] --> B[Declaración: function f(){}]
    A --> C[Expresión: const f = function(){}]
    A --> D[Arrow: const f = () => {}]
    B --> E[Hosting]
    C --> F[No hosting]
    D --> G[this léxico]
    B --> H[Usar return]
    C --> H
```
📖 [[Funciones|Ir al módulo]]

---

### Flujo del Programa
**Lecciones:** 7

```mermaid
graph TD
    A[Control de Flujo] --> B[if / else if / else]
    A --> C[switch / case]
    B --> D[Condición booleana]
    C --> E[Comparación estricta]
    B --> F[Ejecuta bloque verdadero]
    C --> G[Ejecuta case coincidente]
```
📖 [[Flujo del Programa|Ir al módulo]]

---

### Bucles
**Lecciones:** 11

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
📖 [[Bucles|Ir al módulo]]

---

### Proyecto: Tienda de Donas
**Lecciones:** 7

📖 [[Proyecto: Tienda de Donas|Ir al módulo]]

---

### POO - Objetos
**Lecciones:** 12

```mermaid
graph TD
    A[Objeto JS] --> B[Propiedades: clave: valor]
    A --> C[Métodos: función()]
    B --> D[Acceso: obj.prop]
    B --> E[Acceso: obj['prop']]
    C --> F[this se refiere al objeto]
    A --> G[Se crean con {}]
```
📖 [[POO - Objetos|Ir al módulo]]

---

### Prototipos
**Lecciones:** 5

```mermaid
graph TD
    A[simba] --> B[Perro.prototype]
    B --> C[Object.prototype]
    C --> D[null]
    A -.->|hereda| B
    B -.->|hereda| C
```
📖 [[Prototipos|Ir al módulo]]

---

### Clases y Encapsulamiento
**Lecciones:** 5

```mermaid
graph TD
    A[Clase] --> B[constructor()]
    A --> C[Métodos]
    A --> D[Getters / Setters]
    A --> E[Subclase: extends]
    E --> F[super() - llama al padre]
    A --> G[#propiedad - Privada]
```
📖 [[Clases y Encapsulamiento|Ir al módulo]]

---

### Manejo de Excepciones y JSON
**Lecciones:** 5

```mermaid
graph LR
    A[JSON] --> B[JavaScript Object Notation]
    B --> C[Formato de intercambio]
    C --> D[fetch() - Obtener]
    C --> E[JSON.stringify() - Enviar]
    C --> F[JSON.parse() - Leer]
```
📖 [[Manejo de Excepciones y JSON|Ir al módulo]]

---

### Eventos
**Lecciones:** 10

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
📖 [[Eventos|Ir al módulo]]

---

### Promesas y Async/Await
**Lecciones:** 7

```mermaid
stateDiagram-v2
    [*] --> Pending
    Pending --> Fulfilled: resolve()
    Pending --> Rejected: reject()
    Fulfilled --> [*]
    Rejected --> [*]
    Note right of Pending: Estado inicial
    
    state "then()" as THEN
    Fulfilled --> THEN
    Rejected --> catch()
```
📖 [[Promesas y Async/Await|Ir al módulo]]

---

### Frameworks y Librerías
**Lecciones:** 3

📖 [[Frameworks y Librerías|Ir al módulo]]

---

### Solicitudes HTTP
**Lecciones:** 6

```mermaid
graph LR
    A[Métodos HTTP] --> B[GET - Obtener]
    A --> C[POST - Crear]
    A --> D[PUT - Actualizar]
    A --> E[PATCH - Modificar parcial]
    A --> F[DELETE - Eliminar]
```
📖 [[Solicitudes HTTP|Ir al módulo]]

---

### Bases de Datos
**Lecciones:** 0

📖 [[Bases de Datos|Ir al módulo]]

---

### Proyecto Final
**Lecciones:** 7

📖 [[Proyecto Final|Ir al módulo]]

---

### Últimos Temas
**Lecciones:** 3

```mermaid
graph TD
    A[módulo A] --> |export| B[funciones/variables]
    C[módulo B] --> |import| B
    A --> D[type='module' en script]
    A --> E[Uso estricto implícito]
```
📖 [[Últimos Temas|Ir al módulo]]

---


## 📈 Progresión Recomendada

```mermaid
graph LR
    A[HTML Básico] --> B[HTML Intermedio]
    B --> C[Variables]
    C --> D[Funciones]
    D --> E[Flujo del Programa]
    E --> F[Bucles]
    F --> G[Proyecto: Tienda de Donas]
    G --> H[POO - Objetos]
    H --> I[Prototipos]
    I --> J[Clases y Encapsulamiento]
    J --> K[Manejo de Excepciones]
    K --> L[Eventos]
    L --> M[Promesas y Async/Await]
    M --> N[Frameworks y Librerías]
    N --> O[Solicitudes HTTP]
    O --> P[Proyecto Final]
    P --> Q[Últimos Temas]
```

---

> 🧑‍💻 Creado por un estudiante eterno, para estudiantes.
