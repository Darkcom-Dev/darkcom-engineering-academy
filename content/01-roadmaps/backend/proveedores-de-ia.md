# Proveedores de IA

Los **proveedores de IA** son las empresas que desarrollan y ofrecen modelos de lenguaje (LLMs) a través de APIs. Cada uno tiene su filosofía, modelos emblemáticos y casos de uso ideales.

```mermaid
flowchart TB
    Proveedores["🏢 Proveedores de IA"] --> OpenAI["🟢 OpenAI<br/>GPT-4o, o3, DALL-E"]
    Proveedores --> Anthropic["🟣 Anthropic<br/>Claude 4 Sonnet/Opus"]
    Proveedores --> Google["🔵 Google<br/>Gemini 2.5 Flash/Pro"]

    Proveedores --> Otros["🔧 Otros"]
    Otros --> Meta["🟦 Meta<br/>Llama (open source)"]
    Otros --> Mistral["🟧 Mistral AI<br/>Mistral, Codestral"]
    Otros --> DeepSeek["🟥 DeepSeek<br/>DeepSeek-V3, R1"]

    style Proveedores fill:#e1f5fe
    style OpenAI fill:#00a67e,color:#fff
    style Anthropic fill:#6b21a8,color:#fff
    style Google fill:#1a73e8,color:#fff
    style Otros fill:#f5f5f5
```

---

## OpenAI

**OpenAI** es la empresa que inició la revolución de los LLMs con ChatGPT. Sus modelos son los más conocidos y usados del mundo.

```mermaid
graph TB
    subgraph OpenAIModels ["🟢 Modelos OpenAI"]
        GPT4o["GPT-4o<br/>🎯 Flagship<br/>Texto + imágenes + audio<br/>Razonamiento multimodal"]
        GPT4oMini["GPT-4o Mini<br/>⚡ Rápido y barato<br/>Para tareas simples<br/>Mejor costo-beneficio"]
        o3["o3<br/>🧠 Razonamiento profundo<br/>Matemáticas, ciencia<br/>Lógica compleja"]
        Dalle["DALL-E 3<br/>🎨 Generación de imágenes"]
        Whisper["🎤 Whisper<br/>Reconocimiento de voz<br/>Transcripción"]
    end

    style OpenAIModels fill:#00a67e,color:#fff
```

### Características clave

- **ChatGPT** — el producto estrella, interfaz conversacional
- **API versátil** — la más usada en integraciones
- **Function calling** — pionero en llamar herramientas desde el modelo
- **Modos** — texto, imágenes, audio, TTS
- **Razonamiento** — modelo o3 para problemas complejos

### Precios aproximados (por millón de tokens)

| Modelo       | Input (por M tokens) | Output (por M tokens) |
| ------------ | -------------------- | --------------------- |
| GPT-4o       | $2.50                | $10.00                |
| GPT-4o Mini  | $0.15                | $0.60                 |
| o3           | $10.00               | $40.00                |

```python
# Ejemplo con OpenAI
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "Eres un experto en Python"},
        {"role": "user", "content": "Explica las diferencias entre listas y tuplas"}
    ]
)
print(response.choices[0].message.content)
```

> **¿Para quién?** Si quieres el modelo más versátil y conocido, con el ecosistema más grande de herramientas y documentación.

---

## Anthropic

**Anthropic** es la empresa creadora de **Claude**, un modelo conocido por su **seguridad, alineación y capacidad de razonamiento**. Fue fundada por ex-empleados de OpenAI.

```mermaid
graph TB
    subgraph AnthropicModels ["🟣 Modelos Anthropic"]
        Sonnet["Claude 4 Sonnet<br/>🎯 Balance perfecto<br/>Velocidad + calidad<br/>Recomendado para TODO"]
        Opus["Claude 4 Opus<br/>🧠 Máxima capacidad<br/>Tareas complejas<br/>Razonamiento profundo"]
        Haiku["Claude 4 Haiku<br/>⚡ Ultra rápido<br/>Tareas simples<br/>Respuesta inmediata"]
    end

    style AnthropicModels fill:#6b21a8,color:#fff
```

### Características clave

- **Seguridad por diseño** — Constitutional AI, entrenado para ser útil y honesto
- **Contexto largo** — hasta 200K tokens (puede leer libros enteros)
- **Tool use / Function calling** — excelente para agentes y herramientas
- **Claude Code** — agente de código en terminal
- **Artefactos** — genera y muestra documentos, código, diagramas en la interfaz

### Filosofía de Claude

```mermaid
flowchart LR
    subgraph Filosofia ["🧠 Filosofía de Claude"]
        Util["Útil<br/>Responde cuando puede"]
        Honesto["Honesto<br/>Dice 'no sé' cuando<br/>no está seguro"]
        Inofensivo["Inofensivo<br/>Rechaza peticiones<br/>dañinas"]
    end

    Util --> Claude["🤖 Claude"]
    Honesto --> Claude
    Inofensivo --> Claude

    style Filosofia fill:#f3e5f5
    style Claude fill:#6b21a8,color:#fff
```

### Precios aproximados (por millón de tokens)

| Modelo           | Input (por M tokens) | Output (por M tokens) |
| ---------------- | -------------------- | --------------------- |
| Claude Sonnet    | $3.00                | $15.00                |
| Claude Opus      | $15.00               | $75.00                |
| Claude Haiku     | $0.25                | $1.25                 |

```python
# Ejemplo con Anthropic
from anthropic import Anthropic

client = Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1000,
    system="Eres un experto en Python",
    messages=[
        {"role": "user", "content": "Explica las diferencias entre listas y tuplas"}
    ]
)
print(response.content[0].text)
```

> **¿Para quién?** Si priorizas la seguridad, quieres un modelo honesto (que admita cuando no sabe), y necesitas contextos muy largos o razonamiento profundo.

---

## Google (Gemini)

**Google DeepMind** desarrolla **Gemini**, una familia de modelos multimodales nativos integrados con el ecosistema Google.

```mermaid
graph TB
    subgraph GeminiModels ["🔵 Modelos Gemini"]
        Ultra["Gemini 2.5 Pro<br/>🧠 Máxima capacidad<br/>1M+ tokens de contexto<br/>Razonamiento avanzado"]
        Flash["Gemini 2.5 Flash<br/>⚡ Rápido y eficiente<br/>Para la mayoría de tareas<br/>Excelente costo-beneficio"]
        Nano["Gemini Nano<br/>📱 Dispositivos móviles<br/>On-device, sin conexión"]
    end

    style GeminiModels fill:#1a73e8,color:#fff
```

### Características clave

- **Contexto masivo** — 1M+ tokens (puede analizar repos enteros)
- **Multimodal nativo** — texto, imágenes, audio, video desde el diseño
- **Google ecosystem** — integración con Google Cloud, Workspace, Search
- **Gemini CLI** — agente de código en terminal
- **Grounding** — puede buscar en Google para respuestas actualizadas

```mermaid
flowchart LR
    Gemini["🔵 Gemini"] --> Text["📝 Texto<br/>Comprensión y generación"]
    Gemini --> Image["🖼️ Imágenes<br/>Análisis y descripción"]
    Gemini --> Audio["🎤 Audio<br/>Transcripción y análisis"]
    Gemini --> Video["🎬 Video<br/>Procesa video completo"]
    Gemini --> Code2["💻 Código<br/>Generación y depuración"]

    style Gemini fill:#1a73e8,color:#fff
```

### Precios aproximados (por millón de tokens)

| Modelo           | Input (por M tokens) | Output (por M tokens) |
| ---------------- | -------------------- | --------------------- |
| Gemini 2.5 Flash | $0.15                | $0.60                 |
| Gemini 2.5 Pro   | $1.25                | $5.00                 |

```python
# Ejemplo con Gemini
import google.generativeai as genai

genai.configure(api_key="tu-api-key")
model = genai.GenerativeModel("gemini-2.5-flash")

response = model.generate_content("Explica las diferencias entre listas y tuplas")
print(response.text)
```

> **¿Para quién?** Si necesitas contextos muy grandes, estás en el ecosistema Google Cloud, o quieres el mejor precio por calidad.

---

## Comparativa

```mermaid
flowchart TB
    Pregunta{"🔍 ¿Qué priorizas?"}

    Pregunta -->|"Versatilidad, ecosistema,<br/>más documentación"| OpenAIG["🟢 OpenAI (GPT-4o)<br/>El estándar de la industria"]
    Pregunta -->|"Seguridad, honestidad,<br/>contextos largos"| AnthroG["🟣 Anthropic (Claude)<br/>El más seguro y honesto"]
    Pregunta -->|"Contexto masivo,<br/>precio, ecosistema Google"| GoogleG["🔵 Google (Gemini)<br/>El más barato y contextual"]
    Pregunta -->|"Open source,<br/>control total, on-premise"| OtherG["🟦 Meta (Llama)<br/>Ejecutas tú mismo"]

    style Pregunta fill:#e1f5fe
    style OpenAIG fill:#00a67e,color:#fff
    style AnthroG fill:#6b21a8,color:#fff
    style GoogleG fill:#1a73e8,color:#fff
    style OtherG fill:#1565c0,color:#fff
```

### Tabla comparativa

| Característica         | OpenAI (GPT-4o)     | Anthropic (Claude)   | Google (Gemini)      |
| ---------------------- | ------------------- | -------------------- | -------------------- |
| **Modelo estrella**    | GPT-4o              | Claude Sonnet 4      | Gemini 2.5 Flash     |
| **Contexto máximo**    | 128K tokens         | 200K tokens          | 1M+ tokens           |
| **Multimodal**         | ✅ Texto + Img + Audio | ✅ Texto + Imágenes | ✅ Texto + Img + Audio + Video |
| **Agente de código**   | ❌ (solo API)       | ✅ Claude Code       | ✅ Gemini CLI        |
| **Open source**        | ❌                  | ❌                   | ❌                   |
| **Precio (input/M)**   | ~$2.50              | ~$3.00               | ~$0.15               |
| **Fortaleza única**    | Ecosistema + ChatGPT| Seguridad + Honestidad| Contexto masivo + precio |

---

## Otros proveedores destacados

```mermaid
graph TB
    Otros2["🔧 Otros proveedores"] --> Meta2["🟦 Meta<br/>Llama 4<br/>✅ Open source<br/>✅ Gratis (local)<br/>⚠️ Requiere hardware"]

    Otros2 --> Mistral2["🟧 Mistral AI<br/>Mistral Large, Codestral<br/>✅ Bueno para código<br/>✅ Open source (small models)"]

    Otros2 --> DeepSeek2["🟥 DeepSeek<br/>V3, R1 (razonamiento)<br/>✅ Muy barato<br/>✅ Buen rendimiento<br/>⚠️ China (latencia)"]

    Otros2 --> Grok2["✖️ xAI (Grok)<br/>✅ Integración X/Twitter<br/>⚠️ Modelo más nicho"]

    style Otros2 fill:#f5f5f5
    style Meta2 fill:#1565c0,color:#fff
    style Mistral2 fill:#ff6d00,color:#fff
    style DeepSeek2 fill:#c62828,color:#fff
    style Grok2 fill:#1a1a1a,color:#fff
```

### ¿Cuándo usar cada uno?

| Proveedor     | Ideal para                                    |
| ------------- | --------------------------------------------- |
| **OpenAI**    | Cualquier tarea, máxima compatibilidad        |
| **Anthropic** | Seguridad, agentes, contextos largos          |
| **Google**    | Contexto masivo, precio, ecosistema Google     |
| **Meta (Llama)** | On-premise, privacidad, sin costo de API  |
| **Mistral**   | Código, modelos ligeros open source            |
| **DeepSeek**  | Razonamiento barato, presupuesto ajustado      |

---

## Cómo elegir un proveedor

```mermaid
flowchart TB
    Start["🤔 ¿Qué necesitas?"]

    Start -->|"Uso general,<br/>más soporte"| Check1["¿Presupuesto?"]
    Check1 -->|"Alto"| GPT["🟢 GPT-4o<br/>El más completo"]
    Check1 -->|"Bajo"| Flash2["🔵 Gemini Flash<br/>Excelente precio"]

    Start -->|"Agente de código<br/>en terminal"| Check2["¿Ecosistema?"]
    Check2 -->|"Anthropic"| Claude2["🟣 Claude Code"]
    Check2 -->|"Google"| GeminiCLI["🔵 Gemini CLI"]

    Start -->|"Privacidad total<br/>on-premise"| Llama2["🟦 Llama 4<br/>Corre en tu servidor"]

    Start -->|"Contextos enormes<br/>(+100K tokens)"| Gemini2["🔵 Gemini 2.5 Pro<br/>1M+ tokens"]

    style Start fill:#e1f5fe
    style GPT fill:#00a67e,color:#fff
    style Flash2 fill:#1a73e8,color:#fff
    style Claude2 fill:#6b21a8,color:#fff
    style GeminiCLI fill:#1a73e8,color:#fff
    style Llama2 fill:#1565c0,color:#fff
    style Gemini2 fill:#1a73e8,color:#fff
```

---

## Resumen visual

```mermaid
graph TB
    Prov["🏢 Proveedores de IA"]
    Prov --> O["🟢 OpenAI<br/>GPT-4o<br/>Versatilidad"]
    Prov --> A["🟣 Anthropic<br/>Claude<br/>Seguridad"]
    Prov --> G["🔵 Google<br/>Gemini<br/>Contexto + Precio"]
    Prov --> M["🟦 Meta<br/>Llama<br/>Open Source"]

    O --> CaractO["✅ Ecosistema más grande<br/>✅ ChatGPT<br/>✅ Function calling"]
    A --> CaractA["✅ Claude Code<br/>✅ Contexto 200K<br/>✅ Honestidad"]
    G --> CaractG["✅ 1M+ tokens<br/>✅ Precio bajo<br/>✅ Ecosistema Google"]
    M --> CaractM["✅ Gratis<br/>✅ Privacidad total<br/>✅ Corre local"]

    style Prov fill:#e1f5fe
    style O fill:#00a67e,color:#fff
    style A fill:#6b21a8,color:#fff
    style G fill:#1a73e8,color:#fff
    style M fill:#1565c0,color:#fff
```

> **Siguiente paso:** Regístrate en las APIs de OpenAI, Anthropic y Google (todas tienen crédito gratis inicial). Prueba el mismo prompt en los tres modelos y compara resultados. Así encontrarás tu favorito.

## Relacionados:
- [[codigo-asistido-por-ia]] #anterior 
- [[patrones-de-integracion]] #siguiente 