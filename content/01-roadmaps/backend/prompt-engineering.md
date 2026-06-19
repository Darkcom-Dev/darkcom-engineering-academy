# Prompt Engineering

El **prompt engineering** es el arte y ciencia de diseñar instrucciones (prompts) para obtener resultados óptimos de modelos de IA. Es la habilidad más importante para trabajar con LLMs.

```mermaid
flowchart TB
    subgraph Niveles ["📈 Evolución del Prompt Engineering"]
        N1["🔤 Prompt básico<br/>Pregunta directa"]
        N2["📝 Prompt estructurado<br/>Contexto + formato"]
        N3["⚙️ Sistema + herramientas<br/>Agentes, MCP, Skills"]
        N4["🧠 Sistemas multi-agente<br/>Varios agentes coordinados"]
    end

    N1 --> N2 --> N3 --> N4

    style Niveles fill:#f5f5f5
    style N1 fill:#ffe0b2
    style N2 fill:#fff3e0
    style N3 fill:#c8e6c9
    style N4 fill:#e8eaf6
```

> **No es "magia", es ingeniería.** Un buen prompt se diseña, se prueba y se itera.

---

## Técnicas de prompting

```mermaid
graph TB
    Tecnicas["🔧 Técnicas de Prompting"] --> Zero["Zero-shot<br/>Pregunta directa"]
    Tecnicas --> Few["Few-shot<br/>Ejemplos incluidos"]
    Tecnicas --> Chain["Chain-of-Thought<br/>Razonamiento paso a paso"]
    Tecnicas --> Role["Role Prompting<br/>Asignar un rol"]
    Tecnicas --> Tree["Tree-of-Thought<br/>Múltiples caminos"]
    Tecnicas --> Structured["Structured Output<br/>JSON / XML / Markdown"]

    style Tecnicas fill:#e1f5fe
    style Zero fill:#fff3e0
    style Few fill:#c8e6c9
    style Chain fill:#fce4ec
    style Role fill:#e8eaf6
    style Tree fill:#f3e5f5
    style Structured fill:#e8f5e9
```

### 1. Zero-shot Prompting

El más básico. Le das una instrucción directa **sin ejemplos**.

```markdown
❌ Mal prompt:
"Dime algo sobre Python"

✅ Buen prompt zero-shot:
"Explica las diferencias entre listas y tuplas en Python.
Incluye: sintaxis, mutabilidad, rendimiento y casos de uso.
Responde en 3 párrafos máximos."
```

### 2. Few-shot Prompting

Incluyes **ejemplos** en el prompt para guiar el formato y tono de la respuesta.

```markdown
Convierte reseñas de usuarios a calificaciones numéricas.

Ejemplo 1:
Reseña: "Este producto es increíble, superó mis expectativas"
Calificación: 5

Ejemplo 2:
Reseña: "Está bien, pero esperaba más calidad"
Calificación: 3

Ahora convierte:
Reseña: "Pésimo, no funciona, quiero mi dinero de vuelta"
Calificación:
```

### 3. Chain-of-Thought (CoT)

Le pides al modelo que **razone paso a paso** antes de dar la respuesta. Mejora drásticamente problemas de lógica y matemáticas.

```mermaid
flowchart LR
    subgraph SinCoT ["🚫 Sin Chain-of-Thought"]
        Q1["P: Si un tren viaja a 80km/h<br/>y otro a 120km/h..."] --> A1["R: 45 minutos"]
        A1 -.->|"❌ Frecuentemente<br/>incorrecto"| Wrong
    end

    subgraph ConCoT ["✅ Con Chain-of-Thought"]
        Q2["P: Si un tren viaja a 80km/h<br/>y otro a 120km/h..."] --> Step1["1. Calculo distancia relativa"]
        Step1 --> Step2["2. Aplico fórmula t = d/v"]
        Step2 --> Step3["3. Convierto a minutos"]
        Step3 --> A2["R: 45 minutos ✅"]
    end

    style SinCoT fill:#ffcdd2
    style ConCoT fill:#c8e6c9
```

```markdown
❌ Sin CoT:
"¿Cuántas 'r' tiene la palabra 'ferrocarril'?"
→ Podría equivocarse

✅ Con CoT:
"¿Cuántas 'r' tiene la palabra 'ferrocarril'?
Piensa paso a paso:
1. Escribo la palabra: f-e-r-r-o-c-a-r-r-i-l
2. Cuento las 'r': posición 3, 4, 8, 9
3. Total: 4
Respuesta: 4"
```

### 4. Role Prompting

Le asignas un **rol o personalidad** al modelo para que adopte un tono, estilo o perspectiva específica.

```markdown
"Actúa como un senior developer con 15 años de experiencia
revisando código. Revisa este PR y señala:
- Problemas de seguridad
- Malas prácticas
- Oportunidades de refactorización
- Sugerencias de mejora

Sé constructivo pero directo."
```

### 5. Formato estructurado

Para aplicaciones (no conversación), pídele al modelo que devuelva datos en un formato específico.

```markdown
Extrae la siguiente información del email
y devuélvela como JSON:

{
    "remitente": "nombre del que envía",
    "asunto": "asunto del email",
    "prioridad": "alta/media/baja",
    "fecha_limite": "fecha si aplica o null",
    "resumen": "resumen en 1 frase"
}

Email:
[pegar email aquí]
```

### Comparativa de técnicas

| Técnica              | Cuándo usarla                          |
| -------------------- | -------------------------------------- |
| **Zero-shot**        | Tareas simples, preguntas directas     |
| **Few-shot**         | Necesitas un formato específico        |
| **Chain-of-Thought** | Razonamiento, matemáticas, lógica      |
| **Role Prompting**   | Necesitas un tono o perspectiva        |
| **Structured Output**| Integración con sistemas (API, JSON)   |
| **Tree-of-Thought**  | Problemas complejos con múltiples caminos |

---

## Agentes

Un **agente de IA** es un sistema que **percibe su entorno, razona, toma decisiones y ejecuta acciones** para lograr un objetivo. Va más allá de responder preguntas: **hace cosas**.

```mermaid
flowchart TB
    User["👤 Usuario da objetivo"] --> Agent["🧠 Agente IA"]

    Agent --> Think["🤔 Piensa / Razona<br/>'¿Qué necesito hacer?'"]
    Agent --> Tools["🛠️ Usa herramientas<br/>Lee archivos, busca web,<br/>ejecuta código, llama APIs"]
    Agent --> Memory["💾 Memoria<br/>Contexto de la conversación,<br/>historial de acciones"]

    Think --> Plan["📋 Planifica pasos"]
    Plan --> Tools
    Tools --> Think
    Think --> Output["📤 Entrega resultado"]

    style User fill:#e1f5fe
    style Agent fill:#c8e6c9
    style Think fill:#fff3e0
    style Tools fill:#fce4ec
    style Memory fill:#e8eaf6
    style Plan fill:#fff9c4
    style Output fill:#e8f5e9
```

### Ciclo de un agente

```mermaid
sequenceDiagram
    participant User as 👤 Usuario
    participant Agent as 🧠 Agente
    participant Tools as 🛠️ Herramientas

    User->>Agent: "¿Cuál es el clima hoy?"

    Agent->>Agent: ① Necesito consultar el clima
    Agent->>Tools: ② LLAMAR get_weather()
    Tools-->>Agent: ③ 🌧️ 18°C, lluvia ligera

    Agent->>Agent: ④ Formatear respuesta
    Agent-->>User: "Hoy estará a 18°C con lluvia ligera. ¡Lleva paraguas! ☂️"
```

### Componentes de un agente

| Componente      | Función                                      |
| --------------- | -------------------------------------------- |
| **LLM**         | Cerebro que razona y decide                  |
| **Herramientas**| Capacidades: leer archivos, buscar, ejecutar |
| **Memoria**     | Contexto de la conversación e historial      |
| **Planificador**| Decide qué pasos seguir y en qué orden       |
| **Bucle**       | Observa → Piensa → Actúa → Observa...        |

---

## MCP (Model Context Protocol)

**MCP** es un protocolo abierto que permite a los modelos de IA **conectarse de forma estándar** con herramientas, APIs y fuentes de datos externas. Es como **USB-C para la IA**: un conector universal.

```mermaid
graph TB
    subgraph SinMCP ["🚫 Sin MCP (cada conexión es única)"]
        LLM1["🧠 LLM"] --> Tool1["🔌 Integración A<br/>(código ad-hoc)"]
        LLM1 --> Tool2["🔌 Integración B<br/>(código ad-hoc)"]
        LLM1 --> Tool3["🔌 Integración C<br/>(código ad-hoc)"]
    end

    subgraph ConMCP ["✅ Con MCP (protocolo estándar)"]
        LLM2["🧠 LLM"] --> MCP["🔌 MCP Protocol"]
        MCP --> T1["📁 Sistema de archivos"]
        MCP --> T2["🗄️ Base de datos"]
        MCP --> T3["🌐 APIs externas"]
        MCP --> T4["🔍 Búsqueda web"]
        MCP --> T5["📧 Email"]
    end

    style SinMCP fill:#ffcdd2
    style ConMCP fill:#c8e6c9
    style MCP fill:#e3f2fd
```

### ¿Cómo funciona MCP?

```mermaid
sequenceDiagram
    participant App as 📱 App (host)
    participant MCP as 🔌 MCP Client
    participant Server as 🖥️ MCP Server
    participant Service as 🌐 Servicio externo

    App->>MCP: Inicia conexión
    MCP->>Server: Conecta al servidor MCP

    Server->>App: Lista de herramientas disponibles:
    Note over App,Server: get_weather(), search_db(), send_email()

    App->>MCP: LLAMAR get_weather("Madrid")
    MCP->>Server: Ejecuta herramienta
    Server->>Service: Consulta API de clima
    Service-->>Server: 🌡️ 25°C
    Server-->>MCP: Resultado
    MCP-->>App: "Madrid: 25°C, soleado"
```

### Ejemplo de servidor MCP

```javascript
// Un servidor MCP simple
import { Server } from "@modelcontextprotocol/sdk";

const server = new Server({
    name: "mi-servidor-mcp",
    version: "1.0.0"
});

server.tool("saludar", 
    { nombre: { type: "string" } },
    async ({ nombre }) => ({
        content: [{ type: "text", text: `¡Hola, ${nombre}!` }]
    })
);
```

### Ecosistema MCP

| Tipo de servidor | Ejemplos                         |
| ---------------- | -------------------------------- |
| **Archivos**     | Leer/escribir archivos locales   |
| **Bases de datos** | PostgreSQL, SQLite, MySQL     |
| **APIs**         | GitHub, Slack, Notion, Gmail     |
| **Búsqueda**     | Web, documentación, vectores     |
| **Ejecución**    | Terminal, scripts, contenedores  |

> **MCP está siendo adoptado por:** Claude Code, OpenCode, Cursor, y cada vez más herramientas. Es el estándar emergente para conectar IA con el mundo real.

---

## Skills

Una **skill** (o habilidad) es un conjunto de **instrucciones + herramientas** que le enseñas a un modelo IA para que realice una tarea específica de manera consistente.

```mermaid
graph LR
    subgraph SkillStructure ["🧩 Anatomía de una Skill"]
        Instructions["📝 Instrucciones<br/>'Eres un experto en Python'"]
        Tools["🛠️ Herramientas<br/>MCP servers, funciones"]
        Examples["📋 Ejemplos<br/>Few-shot, formatos"]
        Rules["⚖️ Reglas<br/>'No uses librerías externas'"]
    end

    Skill["🎯 Skill"] --> Instructions
    Skill --> Tools
    Skill --> Examples
    Skill --> Rules

    style SkillStructure fill:#f5f5f5
    style Skill fill:#c8e6c9
```

### Ejemplo: Skill para revisión de código

```markdown
# Skill: Code Reviewer

Eres un revisor de código senior. Al recibir un PR:

1. Revisa en este orden:
   a. Seguridad (inyecciones, auth, datos sensibles)
   b. Rendimiento (N+1, bucles, memoria)
   c. Legibilidad (nombres, estructura)
   d. Tests (cobertura, casos borde)

2. Formatea cada issue como:
   - 🔴 CRÍTICO: [descripción]
   - 🟡 ADVERTENCIA: [descripción]
   - 💡 SUGERENCIA: [descripción]

3. Reglas:
   - Siempre sugiere una solución
   - Sé respetuoso y constructivo
   - No señales problemas de estilo menores
```

### ¿Para qué sirven las skills?

- **Consistencia** — el modelo se comporta igual cada vez
- **Especialización** — experto en una tarea específica
- **Compartibles** — puedes compartir skills con tu equipo
- **Iterables** — mejoras la skill, mejora el resultado

---

## Roadmap de Agentes de IA

```mermaid
flowchart TB
    subgraph Roadmap ["🗺️ Roadmap: De usuario a creador de agentes"]
        L1["① Prompt básico<br/>Saber hablar con IA"]
        L2["② Técnicas de prompting<br/>Zero-shot, CoT, few-shot"]
        L3["③ Agentes existentes<br/>Usar Claude Code, OpenCode, Cursor"]
        L4["④ Skills personalizadas<br/>Crear instrucciones reutilizables"]
        L5["⑤ MCP servers<br/>Conectar herramientas propias"]
        L6["⑥ Agentes personalizados<br/>Crear tu propio agente"]
        L7["⑦ Multi-agente<br/>Varios agentes coordinados"]
    end

    L1 --> L2 --> L3 --> L4 --> L5 --> L6 --> L7

    style Roadmap fill:#f5f5f5
    style L1 fill:#ffe0b2
    style L2 fill:#fff3e0
    style L3 fill:#c8e6c9
    style L4 fill:#e3f2fd
    style L5 fill:#fce4ec
    style L6 fill:#e8eaf6
    style L7 fill:#f3e5f5
```

### Niveles de madurez

```markdown
Nivel 1 — Consumidor
├── Sabes pedirle a ChatGPT/Claude lo que necesitas
└── Usas autocompletado básico

Nivel 2 — Power User
├── Usas few-shot, CoT, role prompting
├── Sabes estructurar prompts para resultados predecibles
└── Usas agentes existentes (Cursor, Claude Code)

Nivel 3 — Constructor
├── Creas tus propias skills y prompts reutilizables
├── Configuras MCP servers para tus herramientas
└── Integras IA en tu flujo de trabajo

Nivel 4 — Ingeniero de Agentes
├── Diseñas arquitecturas multi-agente
├── Creas agentes autónomos para tareas complejas
└── Contribuyes al ecosistema (skills, MCP, herramientas)
```

### Habilidades clave

```mermaid
graph TB
    HK["🧠 Habilidades clave"] --> PE["✍️ Prompt Engineering<br/>Escribir instrucciones efectivas"]
    HK --> Arch["🏗️ Arquitectura<br/>Diseñar sistemas multi-agente"]
    HK --> Tools2["🛠️ Herramientas<br/>MCP, APIs, integraciones"]
    HK --> Eval["📊 Evaluación<br/>Medir calidad del output"]
    HK --> Iter["🔄 Iteración rápida<br/>Probar, medir, mejorar"]

    style HK fill:#e1f5fe
    style PE fill:#fff3e0
    style Arch fill:#c8e6c9
    style Tools2 fill:#fce4ec
    style Eval fill:#e8eaf6
    style Iter fill:#e8f5e9
```

---

## Buenas prácticas generales

```mermaid
flowchart LR
    subgraph Do ["✅ SÍ hacer"]
        D1["✔️ Ser específico y detallado"]
        D2["✔️ Dar contexto relevante"]
        D3["✔️ Usar ejemplos concretos"]
        D4["✔️ Iterar basado en resultados"]
        D5["✔️ Probar diferentes enfoques"]
    end

    subgraph Dont ["❌ NO hacer"]
        Dt1["✖️ Ser vago o ambiguo"]
        Dt2["✖️ Asumir que el modelo sabe"]
        Dt3["✖️ Confiar en el primer resultado"]
        Dt4["✖️ Prompts demasiado largos sin estructura"]
    end

    style Do fill:#c8e6c9
    style Dont fill:#ffcdd2
```

---

## Resumen visual

```mermaid
graph TB
    PE2["📝 Prompt Engineering"] --> Tecnicas2["🔧 Técnicas<br/>Zero-shot, CoT,<br/>Few-shot, Role"]
    PE2 --> Agentes2["🤖 Agentes<br/>Razonan + Actúan<br/>+ Herramientas"]
    PE2 --> MCP2["🔌 MCP<br/>Protocolo universal<br/>para herramientas"]
    PE2 --> Skills2["🧩 Skills<br/>Instrucciones<br/>reutilizables"]
    PE2 --> Road2["🗺️ Roadmap<br/>Consumidor →<br/>Ingeniero de agentes"]

    Tecnicas2 --> Mejores["🎯 Mejores prácticas<br/>Sé específico<br/>Itera rápido<br/>Prueba variantes"]

    style PE2 fill:#e1f5fe
    style Tecnicas2 fill:#fff3e0
    style Agentes2 fill:#c8e6c9
    style MCP2 fill:#fce4ec
    style Skills2 fill:#e8eaf6
    style Road2 fill:#f3e5f5
    style Mejores fill:#e8f5e9
```

> **Siguiente paso:** Practica las técnicas de prompting con Claude/GPT. Luego experimenta con skills en OpenCode o Cursor, y finalmente construye tu propio MCP server para conectar tus herramientas favoritas.

## Relacionados:
- [[codigo-asistido-por-ia]] #anterior 