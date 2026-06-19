# Día 1 — Introducción a Agentes de IA

Basado en el *Whitepaper Companion Podcast* del curso intensivo de 5 días de Google × Kaggle.

---

## ¿Qué es un Agente de IA?

> Un agente de IA es un sistema autónomo y orientado a objetivos que **planea, actúa y resuelve problemas complejos en múltiples pasos** sin supervisión constante.

```mermaid
flowchart LR
    A["🤖 IA Pasiva<br/>Responde preguntas<br/>Traduce texto<br/>Genera contenido"] --> B["🧠 IA Autónoma (Agente)<br/>Planifica<br/>Actúa<br/>Resuelve problemas<br/>en múltiples pasos"]
    style A fill:#bbdefb
    style B fill:#c8e6c9
```

---

## Anatomía de un Agente — Las 3 Partes Centrales

```mermaid
flowchart TB
    Agente["🧠 AGENTE"] --> Modelo["🧠 Modelo<br/>El cerebro"]
    Agente --> Herramientas["🛠️ Herramientas<br/>Las manos"]
    Agente --> Orquestacion["🎭 Orquestación<br/>El director"]

    Modelo --> ML[LLM / Motor de razonamiento]
    Modelo --> CM[Gestión de contexto<br/>Decide qué es importante]

    Herramientas --> APIs[APIs externas]
    Herramientas --> BD[Bases de datos]
    Herramientas --> VS[Vectores / RAG]
    Herramientas --> FX[Funciones de código]

    Orquestacion --> Loop[Bucle operativo<br/>Planificar → Actuar → Observar]
    Orquestacion --> Memoria[Memoria y estado]
    Orquestacion --> Estrategia[Estrategia de razonamiento<br/>CoT, ReAct]

    style Agente fill:#e1f5fe,stroke:#333
    style Modelo fill:#bbdefb
    style Herramientas fill:#c8e6c9
    style Orquestacion fill:#fff9c4
```

---

### 1. Modelo — El Cerebro

El LLM central actúa como **motor de razonamiento**. Su función clave es la **gestión de la ventana de contexto**: decide qué información es relevante en cada momento, curando la entrada para el siguiente paso del razonamiento.

```
El modelo recibe información de:
  - La misión/objetivo (prompt inicial)
  - La memoria (estado de la tarea)
  - Las herramientas (resultados de acciones previas)

Y decide: ¿qué es importante ahora? ¿qué ignorar? ¿qué paso sigue?
```

### 2. Herramientas — Las Manos

Son la **conexión con el mundo exterior**. Sin herramientas, el modelo solo puede usar su conocimiento pre-entrenado.

| Tipo | Ejemplos |
|------|----------|
| **APIs** | Calendario, CRM, clima, pagos |
| **Bases de datos** | Consultas SQL, NL2SQL |
| **RAG / Vectores** | Búsqueda en documentos no estructurados |
| **Ejecución de código** | Python sandbox para tareas dinámicas |
| **Búsqueda web** | APIs de búsqueda |

El modelo **razona sobre qué herramienta necesita** para el paso actual de su plan, y la orquestación ejecuta la llamada.

### 3. Orquestación — El Director

Es el **gobernador de todo el proceso**. Gestiona:

- **El bucle operativo** (pensar → actuar → observar)
- **La planificación** (desglosar el objetivo en pasos)
- **La memoria** (corto y largo plazo)
- **La estrategia de razonamiento** (CoT, ReAct)
- **La identidad y reglas del agente** (system prompt / constitución)

```mermaid
flowchart TB
    subgraph "Bucle del Agente (ReAct)"
        P["(1) Obtener<br/>Misión"] --> S["(2) Escanear<br/>Herramientas disponibles"]
        S --> TH["(3) Pensar<br/>'¿Qué hacer ahora?'"]
        TH --> A["(4) Actuar<br/>Llamar herramienta"]
        A --> O["(5) Observar<br/>Resultado"]
        O --> TH
    end
    O --> Done["🎯 ¿Misión<br/>completa?"]
    Done -->|Sí| Final["✅ Respuesta final"]
    Done -->|No| TH
    style P fill:#e3f2fd
    style S fill:#bbdefb
    style TH fill:#fff9c4
    style A fill:#c8e6c9
    style O fill:#e8eaf6
    style Done fill:#ffcdd2
    style Final fill:#81c784,stroke:#333
```

---

## Taxonomía de Capacidades del Agente (5 Niveles)

```mermaid
flowchart TB
    L0["Nivel 0<br/>LLM básico"] --> L1["Nivel 1<br/>+ Herramientas"]
    L1 --> L2["Nivel 2<br/>+ Ingeniería de contexto"]
    L2 --> L3["Nivel 3<br/>+ Colaboración multi-agente"]
    L3 --> L4["Nivel 4<br/>+ Auto-evolución"]

    style L0 fill:#e3f2fd
    style L1 fill:#bbdefb
    style L2 fill:#fff9c4
    style L3 fill:#fce4ec
    style L4 fill:#e8eaf6
```

### Nivel 0 — LLM Base

El modelo de lenguaje por sí solo. Sin herramientas. Solo sabe lo que aprendió en su entrenamiento.

```
Puede: explicar conceptos, responder preguntas históricas
No puede: consultar datos en tiempo real, ejecutar acciones, acceder a APIs
```

### Nivel 1 — Solucionador Conectado

El LLM + herramientas. Se conecta al mundo exterior mediante APIs, bases de datos, búsqueda.

```
Puede: responder el resultado de un partido (busca en web),
        consultar disponibilidad de productos (API de inventario)
Clave: ahora tiene conciencia del mundo real
```

### Nivel 2 — Solucionador Estratégico

Va más allá de tareas simples. Puede manejar **metas complejas de múltiples pasos** usando **ingeniería de contexto**: usa la salida de un paso para dar forma inteligente a la entrada del siguiente paso.

```
Ejemplo: "Busca un buen café a medio camino entre estas dos direcciones"
  1. Usa herramienta de mapas → calcula coordenadas del punto medio
  2. Usa esas coordenadas → busca cafeterías con rating > 4.0
  3. Síntesis: recomienda las mejores opciones

Clave: gestión activa de contexto para evitar ruido
```

### Nivel 3 — Colaboración Multi-Agente

Un **equipo de agentes especializados** que se tratan entre sí como herramientas. Un agente director delega objetivos a agentes subordinados.

```
Ejemplo: "Analiza los precios de la competencia"
  Agente Director → delega a:
    ├── Agente de Investigación de Mercado
    └── Agente de Análisis de Datos

  Cada sub-agente ejecuta su propio plan de varios pasos
  con sus propias herramientas y devuelve un resultado sintetizado.

Clave: delegación de objetivos (no solo llamadas a funciones)
```

### Nivel 4 — Sistema Auto-Evolutivo

El sistema puede **identificar carencias en sus propias capacidades** y crear nuevas herramientas para llenarlas.

```
Ejemplo: El agente director necesita análisis de sentimiento en redes sociales.
         No existe un agente/herramienta que lo haga.
         → Invoca una herramienta de creación de agentes
         → Construye un nuevo agente de análisis de sentimiento sobre la marcha
         → Configura sus permisos y lo integra

Clave: adaptación y expansión autónoma del conjunto de herramientas
```

---

## Construcción de Agentes Confiables

### Selección del Modelo

No siempre el modelo más grande es el mejor para un agente. Se necesita:

- **Buen razonamiento** para planificar
- **Uso confiable de herramientas** (function calling)
- Estrategia de **enrutamiento de modelos**:

```mermaid
flowchart LR
    Tarea["Tarea entrante"] --> Router["🔀 Enrutador"]
    Router -->|"Compleja/alto riesgo"| Pro["💎 Gemini 1.5 Pro<br/>Planificación, decisiones"]
    Router -->|"Simple/alto volumen"| Flash["⚡ Gemini 1.5 Flash<br/>Resúmenes, extracción"]
    style Tarea fill:#bbdefb
    style Router fill:#fff9c4
    style Pro fill:#c8e6c9
    style Flash fill:#e8eaf6
```

### Herramientas Clave

| Técnica | Propósito |
|---------|-----------|
| **RAG** | Fundamentar al agente en hechos (documentos no estructurados) |
| **NL2SQL** | Consultar bases de datos estructuradas en lenguaje natural |
| **APIs** | Acciones concretas (agendar, actualizar CRM, ejecutar código) |
| **Function Calling** | Descripción clara de herramientas (especificación OpenAPI) |

### Function Calling

Cada herramienta necesita una **especificación clara** (como OpenAPI) que le diga al modelo:

```
Qué hace la herramienta
Qué parámetros necesita (ej: orden, email)
Qué formato tendrá la salida
```

Sin esta comunicación estructurada, **todo el bucle puede romperse**.

La herramienta devuelve una **observación** → se alimenta de vuelta al contexto del modelo → listo para el siguiente ciclo.

---

## Memoria del Agente

```mermaid
flowchart TB
    Memoria["🧠 Memoria del Agente"] --> CP["Corto Plazo<br/>Pad de tarea actual"]
    Memoria --> LP["Largo Plazo<br/>Persiste entre sesiones"]

    CP --> Acciones["Historial de acciones<br/>y observaciones<br/>del bucle actual"]

    LP --> Preferencias["Preferencias del usuario"]
    LP --> Pasadas["Interacciones pasadas"]
    LP --> Conocimiento["Conocimiento adquirido"]

    LP -.-> RAG["Implementado como<br/>otra herramienta + RAG<br/>+ base de datos vectorial"]
    style Memoria fill:#e1f5fe,stroke:#333
    style CP fill:#bbdefb
    style LP fill:#c8e6c9
```

---

## Operaciones: Pruebas y Depuración

Los agentes presentan un **desafío único** para testing: la salida puede ser perfectamente válida incluso si está redactada de forma diferente cada vez.

### LM as Judge (LM como Juez)

```mermaid
flowchart LR
    Agente["🤖 Agente<br/>genera respuesta"] --> Judge["⚖️ LM como Juez<br/>otro modelo poderoso"]
    Rubrica["📋 Rúbrica detallada"] --> Judge
    Judge --> Eval{"¿Cumple?"}
    Eval -->|"Sí"| Pass["✅ Válido"]
    Eval -->|"No"| Fail["❌ Rechazado"]
    style Agente fill:#bbdefb
    style Judge fill:#fff9c4
    style Eval fill:#ffcdd2
    style Pass fill:#c8e6c9
    style Fail fill:#ffcdd2
```

**La rúbrica evalúa:**
- ¿La respuesta cumple los requisitos?
- ¿Está fundamentada en hechos?
- ¿Siguió las restricciones negativas?

### Observabilidad con Trazas

```mermaid
flowchart TB
    subgraph "Traza de Agente (caja negra)"
        Paso1["Paso 1<br/>Prompt: ...<br/>Razonamiento: ...<br/>Herramienta: get_team_list<br/>Params: {id: 123}<br/>Observación: [Alice, Bob]"] --> Paso2
        Paso2["Paso 2<br/>Prompt: ...<br/>Razonamiento: ...<br/>Herramienta: check_availability<br/>Params: {team: [Alice, Bob]}<br/>Observación: [Lunes, Miércoles]"]
    end
    style Paso1 fill:#e3f2fd
    style Paso2 fill:#bbdefb
```

Cada traza proporciona un **registro paso a paso**:
- El prompt en cada paso
- El razonamiento del modelo
- Qué herramienta fue elegida
- Los parámetros exactos enviados
- La observación recibida

### Ciclo de Mejora Continua

```
1. Usuario reporta fallo o comportamiento extraño
2. Capturas el escenario
3. Lo reproduces
4. Lo conviertes en un caso de prueba permanente
5. → Vacunas al sistema contra ese error específico
```

---

## Seguridad y Gobernanza

### La Tensión Fundamental

> **Más utilidad → más riesgo potencial**

### Defensa en Profundidad

```mermaid
flowchart TB
    Input["👤 Usuario"] --> G1["Capa 1<br/>Barandillas de código<br/>Políticas de gasto,<br/>reglas duras"]
    G1 --> G2["Capa 2<br/>Modelo de guardia<br/>Detecta acciones<br/>de riesgo antes de ejecutar"]
    G2 --> G3["Capa 3<br/>Identidad del agente<br/>Pasaporte digital<br/>Permisos por rol"]
    G3 --> Accion["⚡ Acción"]
    G1 -.->|"Bloquea si excede límite"| Block["🚫 Bloqueado"]
    G2 -.->|"¿Filtrar datos?<br/>¿Acción prohibida?"| Block
    style Input fill:#bbdefb
    style G1 fill:#c8e6c9
    style G2 fill:#fff9c4
    style G3 fill:#fce4ec
    style Accion fill:#81c784,stroke:#333
    style Block fill:#ffcdd2
```

### Identidad del Agente

> Un agente **no actúa como el usuario**. Es un nuevo actor en el sistema con su propia **identidad segura y verificable** (pasaporte digital, ej: estándares como SEBGF).

```
Identidad permite:
  - Privilegio mínimo: el agente de ventas accede al CRM, NO a RR.HH.
  - Trazabilidad: toda acción queda registrada contra el agente
  - Aislamiento: permisos granulares por rol
```

### Expansión de Agentes (Agent Sprawl)

Cuando escalas a nivel 3+ con decenas/cientos de agentes:

```mermaid
flowchart TB
    Gateway["🚪 Puerta de enlace /<br/>Plano de control central"] --> Policy["Aplica políticas"]
    Gateway --> Auth["Maneja autenticación"]
    Gateway --> Monitor["Panel único de monitoreo<br/>logs, métricas, trazas"]
    Gateway --> Agent1["🤖 Agente 1"]
    Gateway --> Agent2["🤖 Agente 2"]
    Gateway --> AgentN["🤖 Agente N"]
    Agent1 --> Tool1["🛠️ CRM"]
    Agent2 --> Tool2["🛠️ Email"]
    AgentN --> Tool3["🛠️ Calendario"]
    style Gateway fill:#e1f5fe,stroke:#333
    style Policy fill:#c8e6c9
    style Auth fill:#fff9c4
    style Monitor fill:#fce4ec
```

Todo el tráfico (usuario→agente, agente→herramienta, agente→agente) debe pasar por esta puerta de enlace.

### Aprendizaje y Evolución

Los agentes necesitan adaptarse o su rendimiento se degrada a medida que el mundo cambia.

```
Fuentes de aprendizaje:
  - Análisis de experiencia en tiempo de ejecución (logs, trazas)
  - Feedback de usuarios
  - Señales externas (políticas actualizadas de la empresa)

Output del feedback:
  - Refinar system prompts
  - Mejorar ingeniería de contexto
  - Optimizar o crear nuevas herramientas
```

### Simulación — El "Gimnasio de Agentes"

Entorno seguro fuera de producción para:
- Simular interacciones
- Usar datos sintéticos
- Involucrar expertos de dominio
- Probar estrés y optimizar colaboración entre agentes
- Sin impacto en usuarios reales

---

## Ejemplos Reales

### Google Co-Scientist (Nivel 3-4)

Sistema para investigación científica que actúa como **colaborador virtual de investigación**:

```
Agente Supervisor → gestiona el proyecto, delega:
  ├── Agente de formulación de hipótesis
  ├── Agente de diseño de experimentos
  └── Agente de análisis de datos

Itera y refina ideas → refleja flujo de trabajo de investigación humana
```

### Alphavolve (Nivel 4)

Sistema que descubre y optimiza algoritmos usando:
- **LLMs** para generar código
- **Proceso evolutivo automatizado** para probar y mejorar

Ha encontrado operaciones más eficientes de centro de datos y formas más rápidas de matemáticas fundamentales (ej: multiplicación de matrices).

> La asociación: la IA genera soluciones (código), los humanos proveen guía experta, métricas de evaluación y aseguran que las soluciones sean comprensibles.

---

## Conclusión

```mermaid
flowchart TB
    Exito["🏆 Agente exitoso en producción"] --> Modelo["🧠 Modelo<br/>Razonamiento +<br/>contexto"]
    Exito --> Herramientas["🛠️ Herramientas<br/>Function calling<br/>+ APIs"]
    Exito --> Orq["🎭 Orquestación<br/>Bucle + memoria<br/>+ estrategia"]
    Exito --> Seguridad["🔒 Seguridad<br/>Defensa profunda<br/>+ identidad"]
    Exito --> Ops["📊 Operaciones<br/>Testing + trazas<br/>+ mejora continua"]

    style Exito fill:#e1f5fe,stroke:#333
    style Modelo fill:#bbdefb
    style Herramientas fill:#c8e6c9
    style Orq fill:#fff9c4
    style Seguridad fill:#fce4ec
    style Ops fill:#e8eaf6
```

> El éxito de un agente no depende solo del modelo más inteligente. Depende del **rigor de ingeniería**: la arquitectura, la gobernanza, la seguridad, las pruebas y la observabilidad. Tu rol evoluciona de **codificador a arquitecto**, de operador a **director** guiando sistemas autónomos.

---

## Navegación

- [[dia-2-agent-tools-y-interoperabilidad-mcps]] → Siguiente: herramientas del agente y MCP
- [[opcional-dia-1-livestream]] → Siguiente: Livestream Q&A del Día 1
- [[5-day-ai-agents-intensive-course-with-google]] ← Volver al índice
