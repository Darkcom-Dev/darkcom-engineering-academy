# Aplicaciones con IA

La IA está transformando la forma en que escribimos, revisamos y mantenemos código. No reemplaza al desarrollador, pero lo hace **significativamente más productivo**.

```mermaid
flowchart TB
    Dev["👨‍💻 Desarrollador"] --> Code["✏️ Escribe código"]
    Code --> AI["🤖 IA Asistente"]

    subgraph Tasks ["🛠️ Tareas que la IA puede hacer"]
        Review["🔍 Revisión de código<br/>Detectar bugs, vulnerabilidades"]
        Refactor["🔧 Refactorización<br/>Mejorar estructura, rendimiento"]
        Docs["📝 Documentación<br/>Generar comentarios, READMEs"]
        Generate["⚡ Generación<br/>Crear código desde descripción"]
        Test["🧪 Tests<br/>Generar casos de prueba"]
        Explain["💬 Explicación<br/>Entender código legacy"]
    end

    AI --> Tasks

    style Dev fill:#e1f5fe
    style Code fill:#fff3e0
    style AI fill:#c8e6c9
    style Tasks fill:#f5f5f5
```

### ¿Cómo funciona un asistente de código IA?

```mermaid
sequenceDiagram
    participant Dev as 👨‍💻 Desarrollador
    participant Editor as 📝 Editor (VS Code)
    participant AI as 🤖 Modelo IA

    Dev->>Editor: Escribe código / selecciona texto
    Editor->>AI: Envía contexto (código alrededor, lenguaje, tipo de tarea)
    AI->>AI: Procesa con el modelo de lenguaje
    AI-->>Editor: Devuelve sugerencia / explicación / refactor
    Editor-->>Dev: Muestra resultado
    Dev->>Dev: Acepta, modifica o rechaza
```

> **La clave:** El desarrollador siempre tiene la última palabra. La IA es copiloto, no piloto.

---

## Revisión de código

La IA puede revisar tu código en busca de **bugs, vulnerabilidades, malas prácticas y mejoras de rendimiento**.

```mermaid
flowchart TB
    PR["🔀 Pull Request abierto"] --> AIReview["🤖 IA Revisa el código"]

    AIReview --> Categorias{"¿Qué detecta?"}

    Categorias --> Bugs["🐛 Bugs lógicos<br/>❌ if (x = 5) en vez de if (x == 5)"]
    Categorias --> Security["🔒 Vulnerabilidades<br/>💉 SQL injection, XSS"]
    Categorias --> Perf["⚡ Rendimiento<br/>🐌 Bucles innecesarios, N+1"]
    Categorias --> Style["🎨 Estilo y convenciones<br/>📏 Naming, formato"]
    Categorias --> Coverage["🧪 Cobertura<br/>❌ Falta de tests para casos borde"]

    AIReview --> Output["📋 Comentarios en el PR"]
    Output --> Dev2["👨‍💻 Desarrollador revisa<br/>y aplica cambios"]

    style PR fill:#e1f5fe
    style AIReview fill:#c8e6c9
    style Categorias fill:#fff3e0
    style Output fill:#e8eaf6
    style Dev2 fill:#e8f5e9
```

### Ejemplo real

```javascript
// ❌ Código con problemas
function getTotal(items) {
    let total = 0;
    for (var i = 0; i < items.length; i++) {
        total += items[i].price * items[i].quantity;
    }

    // 🐛 Bug: dividir por cantidad para obtener total?
    // 😱 Si quantity es 0, esto explota
    return total / items.length;
}

// 🤖 IA sugiere:
// 1. Bug: el nombre sugiere "total" pero devuelve "promedio"
// 2. Seguridad: si items.length es 0, crash
// 3. Mejora: usar reduce en vez de for + var
```

```javascript
// ✅ Código corregido
function getAverage(items) {
    if (!items?.length) return 0;

    const total = items.reduce((sum, item) => {
        return sum + (item.price * item.quantity);
    }, 0);

    return total / items.length;
}
```

### Herramientas de revisión con IA

| Herramienta      | Cómo funciona                          |
| ---------------- | -------------------------------------- |
| **Copilot Review** | Comentarios automáticos en PRs (GitHub) |
| **CodeRabbit**   | Revisión automatizada de PRs           |
| **Cursor**       | Editor con IA integrada para revisiones |
| **Qodo (Codium)**| Genera tests y revisa PRs              |

---

## Refactorizaciones

La IA puede **mejorar la estructura del código** sin cambiar su comportamiento: renombrar variables, extraer funciones, simplificar condicionales.

```mermaid
flowchart LR
    subgraph Antes ["🚫 Código original"]
        A["function p(x, y, z) {<br/>    let r = 0;<br/>    for(let i=0; i<x.length; i++){<br/>        if(x[i].s == y){<br/>            r += x[i].v * z;<br/>        }<br/>    }<br/>    return r;<br/>}"]
    end

    subgraph Despues ["✅ Refactorizado"]
        B["function calcularTotalPedidos(<br/>    pedidos, estado, descuento<br/>) {<br/>    return pedidos<br/>        .filter(p => p.estado === estado)<br/>        .reduce((total, p) => <br/>            total + p.valor * descuento, 0<br/>        );<br/>}"]
    end

    AI["🤖 IA"] -->|"Nombres claros, filter + reduce,<br/>eliminar variable temporal"| Anteas
    Antes --> Despues

    style Antes fill:#ffcdd2
    style Despues fill:#c8e6c9
    style AI fill:#fff3e0
```

### Tipos de refactorización que la IA hace bien

| Tipo                   | Ejemplo                                      |
| ---------------------- | -------------------------------------------- |
| **Renombrar**          | `x` → `pedidos`, `p` → `producto`            |
| **Extraer función**    | Bloque de 20 líneas → `calcularDescuento()`  |
| **Simplificar**        | `if (x == true)` → `if (x)`                  |
| **Modernizar**         | `var` → `let/const`, `for` → `map/filter`    |
| **Eliminar duplicación** | Código repetido → función reutilizable    |
| **Mejorar rendimiento** | Bucles anidados → objetos lookup            |

### Prompt típico para refactorizar

```
Refactoriza este código JavaScript:
- Usa nombres descriptivos
- Reemplaza for con métodos funcionales (map, filter, reduce)
- Extrae la lógica de descuento a una función separada
- Usa arrow functions
- Añade manejo de errores

[CÓDIGO AQUÍ]
```

---

## Generación de documentación

Una de las tareas que más tiempo consume (y que menos nos gusta) es documentar. La IA puede generar documentación automáticamente a partir del código.

```mermaid
flowchart TB
    Source["📄 Código fuente"] --> AI2["🤖 IA"]
    AI2 --> Comment["💬 Comentarios inline<br/>Explican funciones y lógica"]
    AI2 --> Readme["📖 README<br/>Descripción del proyecto, setup, uso"]
    AI2 --> API["🔌 Documentación de API<br/>Endpoint, params, respuestas"]
    AI2 --> Changelog["📋 Changelog<br/>Resumen de cambios entre versiones"]
    AI2 --> Wiki["📚 Wiki técnica<br/>Arquitectura, decisiones"]

    style Source fill:#e1f5fe
    style AI2 fill:#c8e6c9
    style Comment fill:#fff3e0
    style Readme fill:#e8f5e9
    style API fill:#fce4ec
    style Changelog fill:#e8eaf6
    style Wiki fill:#f3e5f5
```

### Ejemplo: documentación de una función

```javascript
// 🤖 IA genera esto automáticamente:

/**
 * Calcula el precio total de un pedido aplicando descuentos
 * y calculando impuestos según la región del cliente.
 *
 * @param {Array<Product>} productos - Lista de productos en el pedido
 * @param {string} codigoDescuento - Código de descuento opcional
 * @param {string} region - Región del cliente (MX, US, ES)
 * @returns {Promise<Object>} { subtotal, descuento, impuestos, total }
 * @throws {Error} Si la región no tiene configuración de impuestos
 *
 * @example
 * const resultado = await calcularTotal([
 *   { id: 1, precio: 100, cantidad: 2 }
 * ], 'DESC10', 'MX');
 * // → { subtotal: 200, descuento: 20, impuestos: 32, total: 212 }
 */
async function calcularTotal(productos, codigoDescuento, region) {
    // ... implementación
}
```

### Ejemplo: README automático

```markdown
# 📦 api-pedidos

API REST para gestión de pedidos de comercio electrónico.

## 🚀 Inicio rápido

```bash
npm install
cp .env.example .env
npm run dev
```

## 📋 Endpoints

| Método | Ruta              | Descripción             |
| ------ | ----------------- | ----------------------- |
| GET    | /api/pedidos      | Listar pedidos          |
| POST   | /api/pedidos      | Crear pedido            |
| GET    | /api/pedidos/:id  | Obtener pedido          |
| PUT    | /api/pedidos/:id  | Actualizar pedido       |

## 🧪 Tests

```bash
npm test        # Tests unitarios
npm run test:e2e  # Tests end-to-end
```
```

### Herramientas de documentación con IA

| Herramienta           | Genera                                 |
| --------------------- | -------------------------------------- |
| **Copilot / Cursor**  | Comentarios inline, JSDoc              |
| **Mintlify**          | Documentación de APIs                  |
| **GitHub Copilot for PRs** | Resúmenes automáticos de PRs      |
| **Swimm**             | Documentación de código que se actualiza sola |

---

## Buenas prácticas al usar IA

```mermaid
flowchart LR
    subgraph Do ["✅ SÍ hacer"]
        D1["✔️ Usar IA para tareas<br/>repetitivas y mecánicas"]
        D2["✔️ Revisar siempre<br/>el código generado"]
        D3["✔️ Dar contexto claro<br/>en los prompts"]
        D4["✔️ Usar IA como<br/>segunda opinión"]
    end

    subgraph Dont ["❌ NO hacer"]
        Dt1["✖️ Copiar y pegar<br/>sin entender"]
        Dt2["✖️ Confiar ciegamente<br/>en la IA"]
        Dt3["✖️ Prompt vago<br/>sin especificar qué quieres"]
        Dt4["✖️ Subir código sensible<br/>(API keys, secretos)"]
    end

    style Do fill:#c8e6c9
    style Dont fill:#ffcdd2
```

### Consejos para prompts efectivos

```markdown
❌ Mal prompt:
"Refactoriza esto"

✅ Buen prompt:
"Refactoriza esta función de Node.js:
- Divide en funciones más pequeñas (máximo 15 líneas cada una)
- Usa async/await en vez de .then()
- Añade manejo de errores con try/catch
- Renombra variables para que sean descriptivas
- Añade comentarios explicando la lógica de negocio"

```

---

## Flujo de trabajo ideal con IA

```mermaid
flowchart TB
    Plan["📋 Planificar<br/>(tú decides qué construir)"]
    Plan --> Implement["✏️ Implementar<br/>(tú escribes + IA completa)"]
    Implement --> Review["🔍 Revisar<br/>(IA detecta problemas)"]
    Review --> Refactor2["🔧 Refactorizar<br/>(IA sugiere mejoras)"]
    Refactor2 --> Document["📝 Documentar<br/>(IA genera docs)"]
    Document --> Test["🧪 Testear<br/>(IA sugiere casos borde)"]
    Test --> Deploy["🚀 Desplegar"]

    Review -.->|"❌ Bug"| Implement
    Refactor2 -.->|"🔄 Cambios grandes"| Review

    style Plan fill:#e1f5fe
    style Implement fill:#fff3e0
    style Review fill:#fce4ec
    style Refactor2 fill:#e8eaf6
    style Document fill:#c8e6c9
    style Test fill:#f3e5f5
    style Deploy fill:#e8f5e9
```

---

## Resumen visual

```mermaid
graph TB
    AI3["🤖 IA para Desarrollo"] --> Review2["🔍 Revisión de código<br/>Bugs, seguridad, estilo"]
    AI3 --> Refactor3["🔧 Refactorización<br/>Estructura, rendimiento"]
    AI3 --> Doc2["📝 Documentación<br/>JSDoc, README, API docs"]
    AI3 --> Test2["🧪 Tests<br/>Generación de casos"]
    AI3 --> Explain2["💬 Explicación<br/>Código legacy, debugging"]

    AI3 --> Dev3["👨‍💻 Tú decides<br/>(la IA sugiere)"]
    Dev3 --> Code3["💻 Código final<br/>revisado y aprobado"]

    style AI3 fill:#e1f5fe
    style Review2 fill:#fce4ec
    style Refactor3 fill:#e8eaf6
    style Doc2 fill:#c8e6c9
    style Test2 fill:#f3e5f5
    style Explain2 fill:#fff3e0
    style Dev3 fill:#e8f5e9
    style Code3 fill:#ffcc80
```

> **Siguiente paso:** Prueba GitHub Copilot o Cursor en tu editor. Pídele que revise un PR, que refactorice una función fea, o que documente tu API. La práctica es la mejor forma de entender sus límites y fortalezas.

## Relacionados:
- [[bases-de-la-ia]] #anterior 
- [[codigo-asistido-por-ia]] #siguiente 