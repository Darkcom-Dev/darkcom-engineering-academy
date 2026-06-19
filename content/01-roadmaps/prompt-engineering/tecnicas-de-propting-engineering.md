# Técnicas de Prompting

Las técnicas de prompting son **patrones y estrategias** para estructurar tus instrucciones y obtener mejores resultados de un LLM. Cada técnica resuelve un tipo de problema diferente.

```mermaid
flowchart TB
    Tecnicas["🧠 Técnicas de Prompting"] --> Basic[Básicas<br/>Zero-Shot, Few-Shot]
    Tecnicas --> Reasoning[De Razonamiento<br/>CoT, Step-Back, ToT]
    Tecnicas --> Advanced[Avanzadas<br/>Self-Consistency, ReAct]
    Tecnicas --> Config[De Configuración<br/>System, Role, Contexto]
    Basic --> Facil["⚡ Rápidas y simples"]
    Reasoning --> Logica["🔍 Para problemas complejos"]
    Advanced --> Robusto["🎯 Más precisión y fiabilidad"]
    Config --> Control["⚙️ Control del comportamiento"]
    style Tecnicas fill:#e1f5fe,stroke:#333
    style Basic fill:#c8e6c9
    style Reasoning fill:#bbdefb
    style Advanced fill:#fff9c4
    style Config fill:#fce4ec
```

---

## Zero-Shot Prompting

Consiste en darle al LLM una **instrucción directa sin ejemplos**. El modelo usa su conocimiento pre-entrenado para responder.

```
Prompt:  "Traduce al inglés: 'Hola, ¿cómo estás?'"
Salida:  "Hello, how are you?"
```

```mermaid
flowchart LR
    P["📝 Prompt directo<br/>Sin ejemplos"] --> LLM["🤖 LLM<br/>Usa conocimiento<br/>pre-entrenado"] --> R["✅ Respuesta"]
    style P fill:#bbdefb
    style LLM fill:#c8e6c9
    style R fill:#e8f5e9
```

| Cuándo usarlo | Ventajas | Limitaciones |
|--------------|----------|--------------|
| Tareas simples y directas | Rápido, económico, sin preparación | Falla en tareas complejas o matizadas |
| Traducciones, resúmenes cortos | Un solo llamado al modelo | Puede ser inconsistente en formato |

```
✅ Buen zero-shot:   "Clasifica este email como 'spam' o 'no spam': {email}"
❌ Mal zero-shot:    "Resuelve este problema matemático complejo sin pasos intermedios"
```

---

## One-Shot / Few-Shot Prompting

Proporcionas **1 o más ejemplos** en el prompt para guiar el formato, tono y estilo de la respuesta.

### One-Shot (1 ejemplo)

```
Prompt:
  Traduce del español al inglés.
  Ejemplo: "Hola" → "Hello"
  Ahora traduce: "¿Dónde está la biblioteca?"

Salida: "Where is the library?"
```

### Few-Shot (varios ejemplos)

```
Prompt:
  Extrae el sentimiento de cada frase:
  Frase: "Me encanta este producto" → Positivo
  Frase: "Es terrible, no funciona" → Negativo
  Frase: "El clima está nublado" → Neutral
  Frase: "La película fue increíble" →

Salida: Positivo
```

```mermaid
flowchart TB
    subgraph Few-Shot
        E1[Ejemplo 1: input → output] --> LLM["🤖 LLM<br/>Aprende del patrón"]
        E2[Ejemplo 2: input → output] --> LLM
        E3[Ejemplo 3: input → output] --> LLM
        Q[Nueva consulta] --> LLM
        LLM --> R[Respuesta<br/>sigue el patrón]
    end
    style E1 fill:#e3f2fd
    style E2 fill:#bbdefb
    style E3 fill:#e3f2fd
    style Q fill:#fff9c4
    style LLM fill:#c8e6c9
    style R fill:#e8f5e9
```

| Técnica | Ejemplos | Cuándo usarla |
|---------|----------|--------------|
| **Zero-Shot** | 0 | Tareas que el modelo ya conoce bien |
| **One-Shot** | 1 | Dar un formato de referencia |
| **Few-Shot** | 2-5 | Guiar un patrón específico de respuesta |
| **Many-Shot** | 5+ | Casos borde, tonos muy específicos |

> 💡 **Regla:** pocos ejemplos (2-3) bien seleccionados > muchos ejemplos mal elegidos.

---

## Step-Back Prompting

Le pides al LLM que **primero piense en un nivel más abstracto** (step-back) antes de responder la pregunta concreta.

```mermaid
flowchart TB
    P["Pregunta concreta<br/>'¿Por qué un imán atrae<br/>al hierro pero no al cobre?'"] --> SB["Paso 1: Step-Back<br/>'¿Cuál es el principio<br/>físico detrás del<br/>magnetismo?'"]
    SB --> C["Paso 2: Contexto general<br/>Explica el electromagnetismo,<br/>dominios magnéticos"]
    C --> R["Paso 3: Respuesta final<br/>Aplica el principio general<br/>al caso específico"]
    style P fill:#ffcdd2
    style SB fill:#fff9c4
    style C fill:#bbdefb
    style R fill:#c8e6c9
```

```
Sin Step-Back:
  P: "¿Por qué un imán atrae al hierro pero no al cobre?"
  R: "Porque el hierro tiene propiedades magnéticas."

Con Step-Back:
  P: "Antes de responder, piensa: ¿Cuál es el principio físico del
      magnetismo? ¿Qué hace que un material sea ferromagnético?
      Ahora responde por qué el imán atrae al hierro pero no al cobre."
  R: "El magnetismo se debe al alineamiento de dominios magnéticos...
      El hierro tiene electrones desapareados que permiten este
      alineamiento. El cobre tiene su capa d llena, por lo que no
      puede alinear sus dominios magnéticos..."
      ↑ Respuesta mucho más fundamentada
```

| Cuándo usarlo | Ejemplo de step-back |
|--------------|---------------------|
| Preguntas de física/ciencia | _"¿Cuál es la ley física relevante?"_ |
| Problemas de matemáticas | _"¿Qué fórmula o teorema aplica aquí?"_ |
| Análisis de código | _"¿Cuál es el patrón de diseño subyacente?"_ |
| Preguntas de historia | _"¿Qué contexto histórico es relevante?"_ |

---

## Chain of Thought (CoT) — Cadena de Pensamiento

Le pides al LLM que **razone paso a paso** antes de dar la respuesta final. Esto mejora drásticamente la precisión en tareas de razonamiento.

```mermaid
flowchart LR
    P["Problema"] --> Paso1["Paso 1<br/>Analizar"] --> Paso2["Paso 2<br/>Desglosar"] --> Paso3["Paso 3<br/>Calcular"] --> R["Respuesta<br/>final"]
    style P fill:#ffcdd2
    style Paso1 fill:#fff9c4
    style Paso2 fill:#bbdefb
    style Paso3 fill:#c8e6c9
    style R fill:#e8f5e9
```

```
Sin CoT:
  P: "Si tengo 12 manzanas y regalo 3, luego compro 5 más,
      ¿cuántas tengo?"
  R: "14 manzanas."  ← puede acertar o fallar sin explicación

Con CoT:
  P: "Si tengo 12 manzanas y regalo 3, luego compro 5 más,
      ¿cuántas tengo? Piensa paso a paso."
  R: "Empiezo con 12 manzanas.
      Regalo 3 → 12 - 3 = 9 manzanas.
      Compro 5 → 9 + 5 = 14 manzanas.
      Respuesta final: 14 manzanas."
```

### CoT Zero-Shot

Simplemente agregas _"Piensa paso a paso"_ al final del prompt.

```
Prompt: "Un rectángulo tiene 20cm de perímetro. Su largo es
          el doble del ancho. ¿Cuánto mide cada lado? Piensa paso a paso."
```

### CoT Few-Shot

Incluyes un ejemplo donde muestras el razonamiento paso a paso.

```
Prompt:
  P: "Si 3 lápices cuestan $15, ¿cuánto cuestan 7 lápices?"
  R: "1 lápiz cuesta $15 ÷ 3 = $5. 7 lápices cuestan 7 × $5 = $35."

  P: "Si 4 cuadernos cuestan $28, ¿cuánto cuestan 9 cuadernos?"
  R:
```

> 🔬 **Impacto:** CoT puede mejorar la precisión en problemas de razonamiento matemático de ~18% a ~80% en modelos avanzados.

---

## Self-Consistency — Auto-Consistencia

Genera **múltiples respuestas** para el mismo prompt (usando temperatura > 0) y luego elige la **más consistente** (por votación).

```mermaid
flowchart TB
    P[Mismo Prompt] --> Gen1["🤖 Generación 1<br/>temperatura = 0.7"] --> R1[Respuesta A]
    P --> Gen2["🤖 Generación 2<br/>temperatura = 0.7"] --> R2[Respuesta B]
    P --> Gen3["🤖 Generación 2<br/>temperatura = 0.7"] --> R3[Respuesta A]
    P --> Gen4["🤖 Generación N<br/>temperatura = 0.7"] --> R4[Respuesta C]
    R1 --> Votación["🗳️ Votación<br/>¿Cuál aparece más?"]
    R2 --> Votación
    R3 --> Votación
    R4 --> Votación
    Votación --> Final["🏆 Respuesta final<br/>La más frecuente"]
    style P fill:#bbdefb
    style Gen1 fill:#e3f2fd
    style Gen2 fill:#bbdefb
    style Gen3 fill:#e3f2fd
    style Gen4 fill:#bbdefb
    style Votación fill:#fff9c4
    style Final fill:#c8e6c9
```

```
Prompt: "¿Cuál es la capital del país donde nació Gabriel García Márquez?"

Generación 1: "Gabriel García Márquez nació en Colombia.
              La capital de Colombia es Bogotá." → Bogotá
Generación 2: "Nació en Colombia (Aracataca). Capital: Bogotá." → Bogotá
Generación 3: "Colombia. Bogotá." → Bogotá
Generación 4: "Gabriel García Márquez era colombiano.
              La capital es Medellín." → Medellín ← error

Votación: Bogotá (3) vs Medellín (1)
Final: ✅ Bogotá
```

| Ventajas | Costo |
|----------|-------|
| Mayor precisión en problemas complejos | Múltiples llamadas al LLM (3-5x más caro) |
| Reduce el ruido de una sola generación | Mayor latencia |
| Ideal para problemas de opción múltiple | No útil para tareas creativas (poesía, etc.) |

> ⚡ **Variante:** también se puede usar con CoT — generas múltiples cadenas de pensamiento y votas la respuesta final más común.

---

## Tree of Thoughts (ToT) — Árbol de Pensamientos

El modelo explora **múltiples líneas de razonamiento en paralelo**, evaluando cada una antes de decidir cuál seguir. Como un "árbol" de pensamientos.

```mermaid
flowchart TB
    P["Problema"] --> Rama1["Rama A<br/>Primer enfoque"]
    P --> Rama2["Rama B<br/>Segundo enfoque"]
    P --> Rama3["Rama C<br/>Tercer enfoque"]
    Rama1 --> Eval1["Evaluación<br/>¿Prometedor?"]
    Rama2 --> Eval2["Evaluación<br/>¿Prometedor?"]
    Rama3 --> Eval3["Evaluación<br/>¿Prometedor?"]
    Eval1 -->|No| Fallo1["❌ Descartado"]
    Eval2 -->|Sí| SubRama["Sub-rama B1<br/>Profundiza"]
    Eval2 --> SubRama2["Sub-rama B2<br/>Otra variante"]
    Eval3 -->|No| Fallo3["❌ Descartado"]
    SubRama --> EvalB1["Evaluación"]
    SubRama2 --> EvalB2["Evaluación"]
    EvalB1 -->|Sí| Solucion["✅ Solución"]
    style P fill:#bbdefb
    style Rama1 fill:#e3f2fd
    style Rama2 fill:#e3f2fd
    style Rama3 fill:#e3f2fd
    style Eval1 fill:#fff9c4
    style Eval2 fill:#fff9c4
    style Eval3 fill:#fff9c4
    style Fallo1 fill:#ffcdd2
    style Fallo3 fill:#ffcdd2
    style SubRama fill:#c8e6c9
    style SubRama2 fill:#c8e6c9
    style Solucion fill:#81c784,stroke:#333
```

**ToT vs CoT:**

```mermaid
flowchart LR
    subgraph "CoT (lineal)"
        A1["Paso 1"] --> A2["Paso 2"] --> A3["Paso 3"] --> AF["Respuesta única"]
    end
    subgraph "ToT (árbol)"
        B1["Paso 1"] --> B2a["Opción A"] --> B3a["Sub A"]
        B1 --> B2b["Opción B"] --> B3b["Sub B1"]
        B2b --> B3c["Sub B2"]
        B1 --> B2c["Opción C"] --> B3d["Sub C"]
        B3b --> BF["Mejor ruta"]
    end
    style A1 fill:#e3f2fd
    style B1 fill:#bbdefb
    style BF fill:#c8e6c9
```

> 🔬 ToT es más potente que CoT pero **significativamente más costoso** en tokens y llamadas. Solo se usa en problemas que lo justifican (acertijos complejos, optimización, planeación).

---

## ReAct — Razonamiento + Acción

Combina **razonamiento** (thinking) con **acciones** (calling tools). El modelo alterna entre pensar y actuar.

```mermaid
flowchart TB
    Q["❓ Pregunta"] --> T["🧠 Thought<br/>'Necesito buscar<br/>el clima en Bogotá'"]
    T --> A["🔧 Action<br/>llamar API de clima"]
    A --> O["📄 Observation<br/>'Bogotá: 18°C'"]
    O --> T2["🧠 Thought<br/>'Ahora convierto a °F'"]
    T2 --> A2["🔧 Action<br/>ejecutar cálculo"]
    A2 --> O2["📄 Observation<br/>'18°C = 64.4°F'"]
    O2 --> R["✅ Response<br/>'Bogotá está a 18°C (64.4°F)'"]
    style Q fill:#bbdefb
    style T fill:#fff9c4
    style T2 fill:#fff9c4
    style A fill:#ffcdd2
    style A2 fill:#ffcdd2
    style O fill:#e8f5e9
    style O2 fill:#e8f5e9
    style R fill:#c8e6c9
```

**Estructura típica de ReAct:**

```
Thought: Necesito averiguar la población de Tokio.
Action: search("población de Tokio 2024")
Observation: Tokio tiene aproximadamente 14 millones de habitantes.
Thought: Ahora necesito compararlo con la población de Nueva York.
Action: search("población de Nueva York 2024")
Observation: Nueva York tiene aproximadamente 8.4 millones.
Thought: Tokio (14M) es significativamente más grande que Nueva York (8.4M).
Response: Tokio tiene ~14M de habitantes, mientras que Nueva York
          tiene ~8.4M. Tokio es casi 1.7 veces más grande.
```

### ReAct vs CoT vs ToT

| Técnica | Razonamiento | Acciones | Uso |
|---------|-------------|----------|-----|
| **CoT** | Secuencial lineal | ❌ No | Problemas de lógica pura |
| **ToT** | Ramificado | ❌ No | Problemas con múltiples caminos |
| **ReAct** | Intercalado | ✅ Sí | Agentes, preguntas que requieren búsqueda |

---

## Prompt Tuning

Técnica más avanzada donde se **aprenden vectores "soft prompt"** mediante optimización, en lugar de escribir prompts manualmente. A diferencia del fine-tuning, **no modifica los pesos del modelo**, solo ajusta el prompt de entrada.

```mermaid
flowchart LR
    subgraph Prompt Tuning
        SP["Soft Prompt<br/>🧪 Vector aprendido"] --> Input["Input<br/>Tokenizado"]
        Input --> Model["Modelo congelado<br/>(pesos fijos)"]
        Model --> Output["Output"]
    end
    SP -.-> Train["Entrenamiento<br/>backpropagation solo<br/>en soft prompt"]
    style SP fill:#ffcdd2
    style Model fill:#c8e6c9
    style Train fill:#e3f2fd
```

| | Prompt Engineering | Prompt Tuning | Fine-Tuning |
|--|-------------------|---------------|-------------|
| **Qué cambia** | El texto del prompt | Vectores aprendidos | Pesos del modelo |
| **Requiere GPU** | ❌ No | ✅ Sí (leve) | ✅ Sí (pesada) |
| **Datos** | ❌ No requiere | ✅ 100-1000 ejemplos | ✅ Miles de ejemplos |
| **Flexibilidad** | Máxima (cambias texto) | Media (requiere re-entreno) | Baja (modelo fijo) |

---

## Sistema / Role / Contexto

Técnicas que controlan **cómo se comporta** el LLM antes de darle la tarea concreta.

### System Prompt

Es la **instrucción base** que define el comportamiento general del modelo. Generalmente no visible para el usuario final.

```
System:
  "Eres un asistente de IA amable, servicial y honesto.
   Si no sabes algo, dilo. No inventes información.
   Responde siempre en español."

User: "¿Qué es la inteligencia artificial?"
```

### Rol Prompting

Le asignas un **rol específico** al modelo para que adopte una perspectiva o estilo.

```
"Actúa como un tutor de matemáticas para niños de 10 años.
 Explica los conceptos de forma simple y con ejemplos divertidos.

 Pregunta: ¿Qué es una fracción?"
```

### Prompt Contextual

Le das **contexto relevante** antes de la pregunta para que la respuesta sea precisa.

```
"Contexto: Eres un agente de soporte técnico para una empresa de software.
 El usuario acaba de comprar una licencia y no puede activarla.

 Historial del ticket: El usuario intentó activar con el código XYZ123
 pero recibe 'código inválido'.

 Pregunta: ¿Qué le dices al usuario?"
```

### ¿Cuál usar?

```mermaid
flowchart TB
    Situación["¿Qué necesitas controlar?"] --> Comportamiento["Comportamiento general"]
    Situación --> Perspectiva["Perspectiva o tono"]
    Situación --> Información["Contexto específico"]

    Comportamiento --> Sys["System Prompt<br/>'Eres honesto,<br/>responde en español'"]
    Perspectiva --> Role["Role Prompting<br/>'Actúa como abogado,<br/>tutor, chef...'"]
    Información --> Context["Contextual Prompt<br/>'Dados estos datos...<br/>responde...'"]

    style Situación fill:#e1f5fe,stroke:#333
    style Comportamiento fill:#bbdefb
    style Perspectiva fill:#c8e6c9
    style Información fill:#fff9c4
    style Sys fill:#e3f2fd
    style Role fill:#e8f5e9
    style Context fill:#fff3e0
```

### Combinación común

La mayoría de las aplicaciones usan **los tres niveles juntos**:

```
System:
  "Eres un asistente útil para desarrolladores. Respondes en español."

User (contexto):
  "El usuario está usando Python 3.12 y tiene este error:
   TypeError: 'int' object is not iterable

   Código:
   for i in 5:
       print(i)"

User (rol + tarea):
  "Actúa como un mentor senior de Python y explica:
   1. Por qué ocurre este error
   2. Cómo solucionarlo
   3. Una analogía para entenderlo"
```

---

## Comparativa: ¿Qué técnica usar?

```mermaid
flowchart TB
    Inicio["¿Qué necesitas hacer?"] --> Simple["Tarea simple<br/>traducir, resumir, clasificar"]
    Inicio --> Formato["Necesito un formato<br/>específico de salida"]
    Inicio --> Razonar["Requiere razonamiento<br/>matemáticas, lógica"]
    Inicio --> Investigar["Requiere búsqueda<br/>o información externa"]
    Inicio --> Precision["Máxima precisión<br/>en respuestas críticas"]

    Simple --> ZeroShot["Zero-Shot"]
    Formato --> FewShot["Few-Shot<br/>con ejemplos"]
    Razonar --> CoT["Chain of Thought"]
    Razonar --> ToT["Tree of Thoughts<br/>(si es muy complejo)"]
    Investigar --> ReAct["ReAct"]
    Precision --> SelfC["Self-Consistency"]
    Precision --> CoT --> SelfC["Self-Consistency<br/>+ CoT"]

    ZeroShot --> SysRole["+ System / Role / Contexto<br/>(siempre que sea posible)"]
    FewShot --> SysRole
    CoT --> SysRole
    ToT --> SysRole
    ReAct --> SysRole
    SelfC --> SysRole

    style Inicio fill:#e1f5fe,stroke:#333
    style Simple fill:#c8e6c9
    style Formato fill:#bbdefb
    style Razonar fill:#fff9c4
    style Investigar fill:#fce4ec
    style Precision fill:#ffcdd2
    style SysRole fill:#e8eaf6
```

---

## Resumen Visual

```mermaid
flowchart TB
    title["📚 Técnicas de Prompting"] --> Básicas
    title --> Razonamiento
    title --> Avanzadas
    title --> Config

    subgraph Básicas
        ZS["Zero-Shot<br/>Sin ejemplos"]
        FS["Few-Shot<br/>Con ejemplos"]
    end

    subgraph Razonamiento
        SB["Step-Back<br/>Abstracto → concreto"]
        COT["Chain of Thought<br/>Paso a paso"]
    end

    subgraph Avanzadas
        SC["Self-Consistency<br/>Votación múltiple"]
        TOT["Tree of Thoughts<br/>Exploración en árbol"]
        RE["ReAct<br/>Pensar + Actuar"]
        PT["Prompt Tuning<br/>Soft prompts aprendidos"]
    end

    subgraph Config
        SP["System Prompt<br/>Comportamiento base"]
        RP["Role Prompting<br/>Perspectiva"]
        CP["Contextual Prompt<br/>Contexto específico"]
    end

    style title fill:#e1f5fe,stroke:#333
    style Básicas fill:#c8e6c9
    style Razonamiento fill:#bbdefb
    style Avanzadas fill:#fff9c4
    style Config fill:#fce4ec
```

---

## Relacionados

- [[elementos-nucleo-llm-prompt-engineering]] ← Anterior: parámetros y configuración del LLM
- [[ingenieria-de-prompts]] → Siguiente: automatización y mejores prácticas
