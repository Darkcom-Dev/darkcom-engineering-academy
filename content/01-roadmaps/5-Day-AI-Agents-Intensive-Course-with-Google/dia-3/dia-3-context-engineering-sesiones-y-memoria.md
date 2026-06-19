# Día 3 — Ingeniería de Contexto, Sesiones y Memoria

*Basado en el Whitepaper Companion Podcast del Día 3 del curso intensivo de 5 días de Google × Kaggle.*

---

## El Problema Central: Los LLM son Stateless

> **Los LLM son fundamentalmente apátridas.** Cada llamada API es como empezar de nuevo. El modelo tiene memoria cero del turno anterior.

```mermaid
flowchart LR
    Call1["Llamada 1<br/>'Hola, soy Juan'"] --> LLM["🤖 LLM<br/>Responde"] --> Call2["Llamada 2<br/>'¿Cómo me llamo?'"]
    Call2 --> LLM
    LLM --> Error["❌ No lo sabe<br/>¡No hay memoria<br/>entre llamadas!"]
    style Call1 fill:#e3f2fd
    style Call2 fill:#bbdefb
    style Error fill:#ffcdd2
```

Para que un agente tenga **estado**, hay que construir dinámicamente un paquete de información complejo en **cada llamada**. Eso es exactamente la **ingeniería de contexto**.

---

## Las 3 Ideas Centrales

```mermaid
flowchart TB
    Title["🧠 Agente con Estado"] --> CE["🎛️ Ingeniería de<br/>Contexto"]
    Title --> SS["📦 Sesiones"]
    Title --> MM["🗄️ Memoria"]

    CE --> Control["Controlador maestro<br/>Gestiona toda la info<br/>que entra al contexto"]
    SS --> Container["Contenedor de la<br/>conversación actual<br/>(banco de trabajo)"]
    MM --> Persistence["Persistencia a<br/>largo plazo entre<br/>sesiones (archivador)"]

    CE --> Dynamic["Captura + gestiona<br/>dinámicamente la<br/>información correcta"]
    SS --> Events["Eventos (historial<br/>cronológico)"]
    SS --> State["Estado estructurado<br/>(carrito, paso actual)"]
    MM --> ETL["ETL cycle:<br/>Extraer → Consolidar<br/>→ Recuperar"]

    style Title fill:#e1f5fe,stroke:#333
    style CE fill:#bbdefb
    style SS fill:#c8e6c9
    style MM fill:#fff9c4
```

---

## Ingeniería de Contexto

No es solo **ingeniería de prompts**. La ingeniería de prompts es darle una receta al chef. La ingeniería de contexto es **prepararle toda la mise en place** (ingredientes correctos, herramientas listas, memoria del usuario).

```mermaid
flowchart LR
    PE["📝 Prompt Engineering<br/>Receta estática"] --> Result["🍳 Plato decente"]
    CE["🎛️ Context Engineering<br/>Mise en place completa"] --> Result2["🍽️ Plato excelente<br/>personalizado"]
    style PE fill:#e3f2fd
    style CE fill:#c8e6c9
```

### ¿Qué incluye la ingeniería de contexto?

```
Datos operativos:
  - Instrucciones del sistema
  - Definiciones de herramientas
  - Ejemplos few-shot

Datos externos:
  - Memoria a largo plazo (recuperada)
  - Documentos vía RAG
  - Salidas de herramientas recién usadas

Datos de la conversación:
  - Historial del diálogo
  - Scratchpad temporal
  - Último mensaje del usuario
```

### El Problema: Context Decay (Podredumbre del Contexto)

> Cuando la ventana de contexto se llena demasiado y se vuelve ruidosa, la capacidad del modelo para prestar atención a las partes importantes y razonar efectivamente **se degrada**.

### El Ciclo de Gestión de Contexto

```mermaid
flowchart TB
    Start["🔄 Por cada turno"] --> Search["(1) Buscar contexto<br/>Memorias relevantes<br/>Documentos RAG"]
    Search --> Prepare["(2) Preparar contexto<br/>🛑 EN RUTA CRÍTICA<br/>Ensamblar el prompt completo"]
    Prepare --> Invoke["(3) Invocar LLM<br/>y herramientas"]
    Invoke --> Upload["(4) Subir contexto<br/>⏳ ASÍNCRONO (background)<br/>Guardar nuevo aprendizaje"]
    Upload --> Start
    style Start fill:#e1f5fe
    style Search fill:#bbdefb
    style Prepare fill:#ffcdd2
    style Invoke fill:#c8e6c9
    style Upload fill:#e8eaf6
```

**Paso 4 crítico:** debe hacerse **de fondo, asincrónicamente**. Nunca debe bloquear la respuesta al usuario.

---

## Sesiones

La sesión es el **banco de trabajo** para una conversación continua. Es autónoma, específica de un usuario, y mantiene el contexto para una sola conversación.

### Estructura de una Sesión

```mermaid
flowchart TB
    Session["📦 Sesión"] --> Events["📋 Eventos<br/>Registro cronológico<br/>Usuario: X → Agente: Y<br/>Agente llama herramienta Z"]
    Session --> State["📌 Estado estructurado<br/>Artículos en carrito<br/>Paso actual en reserva<br/>Datos temporales"]
    style Session fill:#e1f5fe,stroke:#333
    style Events fill:#bbdefb
    style State fill:#c8e6c9
```

### Manejo de Sesiones por Framework

| Framework | Modelo de Sesión | Clave |
|-----------|------------------|-------|
| **ADK** | Objeto explícito con eventos + estados separados | Clara separación de concerns |
| **LangGraph** | Objeto de estado **mutable** como sesión | Permite compactación directa (reemplazar mensajes antiguos con resúmenes) |

### Sesiones en Sistemas Multi-Agente

```mermaid
flowchart TB
    subgraph "Historia Compartida"
        A1["🤖 Agente 1"] --> Log["📋 Log central<br/>compartido"]
        A2["🤖 Agente 2"] --> Log
        A3["🤖 Agente 3"] --> Log
        Log --> Todos["Todos ven todo<br/>👍 Rico contexto compartido<br/>👎 Abarrotado, difícil trazabilidad"]
    end
    subgraph "Historias Separadas"
        B1["🤖 Agente 1"] --> M1["📋 Su propia historia"]
        B2["🤖 Agente 2"] --> M2["📋 Su propia historia"]
        B1 -.->|"Mensajes explícitos"| B2
        B2 -.->|"Mensajes explícitos"| B1
        Label["👍 Autonomía<br/>👎 Pierde contexto compartido"]
    end
    style A1 fill:#bbdefb
    style A2 fill:#c8e6c9
    style A3 fill:#e8eaf6
    style B1 fill:#bbdefb
    style B2 fill:#c8e6c9
    style Log fill:#fff9c4
```

### Consideraciones de Producción

| Aspecto | Práctica |
|---------|----------|
| **Privacidad** | Aislamiento estricto (ACLs). Redactar PII **antes** de almacenar (no después) |
| **Cumplimiento** | GDPR, CCPA — sanitizar con herramientas como DLP |
| **TTL** | Políticas de tiempo de vida para sesiones |
| **Orden** | Los eventos deben ser deterministas |
| **Rendimiento** | Los datos de sesión están en la ruta activa → la compactación es crítica |

---

## Compactación de Contexto

> Como un viajero empacando una maleta. No puedes meter todo. Golpeas los límites de la API, los costos suben, la latencia empeora, la calidad baja por **context decay**.

### Estrategias de Compactación

```mermaid
flowchart TB
    Compactacion["📦 Compactación"] --> Simple["Simples"]
    Compactacion --> Avanzada["Avanzadas"]

    Simple --> S1["Ventana deslizante<br/>Solo últimos N turns"]
    Simple --> S2["Truncamiento por tokens<br/>Contar tokens desde el<br/>mensaje más nuevo"]

    Avanzada --> A1["Resumen recursivo<br/>LLM resume partes<br/>antiguas del historial"]

    A1 --> Detalle["Periódicamente:<br/>1. Toma historial antiguo<br/>2. Pide al LLM resumir<br/>3. Reemplaza original con resumen<br/>4. Prefija turns textuales recientes"]

    style Compactacion fill:#e1f5fe,stroke:#333
    style Simple fill:#bbdefb
    style Avanzada fill:#c8e6c9
    style A1 fill:#fff9c4
```

**Resumen recursivo:**
- ✅ Conserva la esencia
- ✅ Reduce drásticamente el conteo de tokens
- ⚠️ Computacionalmente pesado → **debe ser asíncrono en background**
- Se activa por: número de turns, período de inactividad, o cuando se completa una tarea

---

## Memoria

### Memoria vs RAG

```mermaid
flowchart TB
    RAG["📚 RAG"] --> Bibliotecario["Investigación bibliotecaria<br/>(información factual estática)"]
    Memoria["🗄️ Memoria"] --> Asistente["Asistente personal<br/>(cosas específicas del usuario<br/>dinámicas)"]
    RAG --> Mundo["Hace al agente<br/>conocedor del mundo"]
    Memoria --> Tu["Hace al agente<br/>conocedor de TI"]
    style RAG fill:#bbdefb
    style Memoria fill:#c8e6c9
```

### Tipos de Memoria

```mermaid
flowchart TB
    Mem["🧠 Memoria"] --> Declarativa["📖 Declarativa<br/>SABER QUÉ"]
    Mem --> Procedural["⚙️ Procedimental<br/>SABER CÓMO"]

    Declarativa --> Hechos["Hechos, eventos, cifras<br/>Ej: equipo favorito del usuario,<br/>ciudad de destino del viaje"]
    Procedural --> Skills["Habilidades, flujos de trabajo<br/>Ej: secuencia exacta de llamadas<br/>para reservar un vuelo"]
    style Mem fill:#e1f5fe,stroke:#333
    style Declarativa fill:#bbdefb
    style Procedural fill:#c8e6c9
```

### Organización de la Memoria

| Estructura | Descripción |
|------------|-------------|
| **Colecciones** | Conjuntos de recuerdos por usuario o tema |
| **Perfil de usuario** | Tarjeta de contacto rápida (core facts) |
| **Resumen acumulativo** | Documento único que evoluciona continuamente |

### Almacenamiento: Híbrido Vectorial + Grafos de Conocimiento

```mermaid
flowchart LR
    Query["🔍 Consulta"] --> Vector["🔢 Búsqueda vectorial<br/>Encuentra cosas como X"]
    Query --> Graph["🕸️ Grafo de conocimiento<br/>Responde cómo X se<br/>relaciona con Y y Z"]
    Vector --> Memorias["Recuerdos relevantes"]
    Graph --> Relaciones["Relaciones entre<br/>recuerdos"]
    style Query fill:#e3f2fd
    style Vector fill:#bbdefb
    style Graph fill:#c8e6c9
    style Memorias fill:#fff9c4
    style Relaciones fill:#e8eaf6
```

### Alcance de la Memoria

| Nivel | Ámbito | Ejemplo |
|-------|--------|---------|
| **Usuario** | Entre sesiones del mismo usuario | Preferencias, datos personales |
| **Sesión** | Solo dentro de un chat | Información temporal de la conversación |
| **Aplicación** | Global (requiere cuidado) | Procedimientos compartidos |

### Generación de Memoria: Ciclo ETL

El proceso de creación de memoria es un proceso **ETL impulsado por LLM**:

```mermaid
flowchart TB
    ETL["🔄 Ciclo ETL de Memoria"] --> Extract["(1) Extracción<br/>Filtrado dirigido"]
    ETL --> Consolidate["(2) Consolidación<br/>Autoedición +<br/>resolución de conflictos"]
    ETL --> Retrieve["(3) Recuperación<br/>Mezcla de puntuaciones"]

    Extract --> Filtro["Filtra lo significativo<br/>según el propósito del agente<br/>(reglas, temas, ejemplos)"]

    Consolidate --> Decision{"¿Qué hacer con<br/>este recuerdo?"}
    Decision --> New["➕ Crear nuevo"]
    Decision --> Update["🔄 Actualizar existente"]
    Decision --> Delete["➖ Eliminar/Invalidar<br/>antiguo"]

    Retrieve --> Scores["Mezcla:<br/>🔤 Relevancia (similaridad)<br/>⏰ Actualidad (recencia)<br/>⭐ Importancia (crítica inicial)"]

    style ETL fill:#e1f5fe,stroke:#333
    style Extract fill:#bbdefb
    style Consolidate fill:#fff9c4
    style Retrieve fill:#c8e6c9
    style Decision fill:#ffcdd2
```

**Procedencia y confianza:**
- ¿De dónde vino el recuerdo?
- ¿Qué edad tiene?
- ¿Fue declarado explícitamente por el usuario o inferido por el agente?
- Las puntuaciones de confianza evolucionan: si el usuario menciona a su perro Winston 3 veces → la confianza sube. Si no se accede en meses → la relevancia decae.

> ⚠️ La generación de memoria **SIEMPRE debe ser asíncrona** (background). Si la consolidación ocurre mientras el usuario espera, la latencia será inaceptable.

---

## Memoria como Herramienta

En lugar de depender solo de reglas predefinidas, le das al agente herramientas para **gestionar su propia memoria**:

```mermaid
flowchart TB
    Agent["🤖 Agente"] --> Tools["🛠️ Herramientas de memoria"]
    Tools --> Create["create_memory<br/>'Esto es importante,<br/>debo guardarlo'"]
    Tools --> Query["query_memory<br/>'Necesito saber las<br/>preferencias del usuario'"]
    Agent --> Decision2{"⚡ Durante la<br/>conversación"}
    Decision2 --> Save["'Oye, este dato<br/>parece importante'"]
    Decision2 --> Check["'Revisemos la<br/>memoria primero'"]
    style Agent fill:#bbdefb
    style Tools fill:#c8e6c9
    style Create fill:#fff9c4
    style Query fill:#e8eaf6
```

### Recuperación: Proactiva vs Reactiva

| Estrategia | Cuándo | Pros | Contras |
|------------|--------|------|---------|
| **Proactiva** | Al inicio de cada turno | Simple, siempre presente | Latencia si es lenta; trae cosas irrelevantes |
| **Reactiva** | Cuando el agente decide consultar | Más eficiente | Requiere agente más inteligente; LLM call extra |

### Colocación de Recuerdos en el Prompt

```mermaid
flowchart TB
    Placement["📌 ¿Dónde poner<br/>los recuerdos?"] --> System["📋 Instrucciones del sistema"]
    Placement --> History["💬 Historial de<br/>conversación"]

    System --> Peso["Mucho peso<br/>👍 Bueno para hechos estables<br/>(perfil de usuario)"]
    System --> Riesgo["👎 Si la memoria es incorrecta,<br/>sesga toda la respuesta"]

    History --> Natural["👍 Flujo más natural"]
    History --> Riesgo2["👎 El LLM puede confundirse<br/>y pensar que el recuerdo<br/>es parte del diálogo actual"]
    style Placement fill:#e1f5fe,stroke:#333
    style System fill:#bbdefb
    style History fill:#c8e6c9
    style Peso fill:#fff9c4
    style Riesgo fill:#ffcdd2
    style Riesgo2 fill:#ffcdd2
```

---

## Evaluación de Sistemas de Memoria

```mermaid
flowchart TB
    Testing["🧪 Evaluación de Memoria"] --> Gen["Métricas de<br/>Generación"]
    Testing --> Ret["Métricas de<br/>Recuperación"]
    Testing --> Lat["Rendimiento"]
    Testing --> E2E["Éxito de Tarea<br/>Extremo a Extremo"]

    Gen --> Precision["Precisión: ¿capturamos<br/>los recuerdos correctos?"]
    Gen --> Recall["Recall: ¿cuántos de los<br/>recuerdos correctos capturamos?"]

    Ret --> RecallK["Recall@K: ¿el recuerdo<br/>correcto aparece en<br/>los primeros K resultados?"]

    Lat --> Speed["< 200ms por búsqueda"]

    E2E --> LLMJudge["Usar LLM como juez<br/>para calificar objetivamente"]
    style Testing fill:#e1f5fe,stroke:#333
    style Gen fill:#bbdefb
    style Ret fill:#c8e6c9
    style Lat fill:#fff9c4
    style E2E fill:#fce4ec
```

---

## Resumen Visual

```mermaid
flowchart TB
    title["🧠 Agentes con Estado"] --> CE["🎛️ Ingeniería de Contexto"]
    title --> SES["📦 Sesiones"]
    title --> MEM["🗄️ Memoria"]

    CE --> Loop["Ciclo por turno:<br/>Buscar → Preparar →<br/>Invocar → Subir (async)"]
    CE --> Decay["Combate context decay<br/>con compactación"]

    SES --> Contain["Banco de trabajo<br/>Eventos + Estado"]
    SES --> Multi["Multi-agente:<br/>Compartida o separada"]
    SES --> Prod["Aislamiento, PII, TTL"]

    MEM --> Types["Declarativa +<br/>Procedimental"]
    MEM --> ETL["Ciclo ETL:<br/>Extraer → Consolidar<br/>→ Recuperar"]
    MEM --> Tools["Memoria como<br/>herramienta"]
    MEM --> Eval["Evaluar precisión,<br/>recall, latencia"]

    style title fill:#e1f5fe,stroke:#333
    style CE fill:#bbdefb
    style SES fill:#c8e6c9
    style MEM fill:#fff9c4
```

> El camino para construir agentes que **aprendan, recuerden y personalicen** está en dominar la interacción entre ingeniería de contexto, sesiones y memoria.

---

## Navegación

- [[dia-2-agent-tools-y-interoperabilidad-mcps]] ← Anterior: herramientas y MCP
- [[dia-4-agent-quality]] → Siguiente: calidad del agente
- [[opcional-dia-3-livestream]] → Siguiente: Livestream Q&A del Día 3
- [[5-day-ai-agents-intensive-course-with-google]] ← Volver al índice
