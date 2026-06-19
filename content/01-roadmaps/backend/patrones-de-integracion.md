# Patrones de integración

Los **patrones de integración** son formas estandarizadas de conectar aplicaciones con modelos de IA. Dominarlos es esencial para construir aplicaciones robustas con LLMs.

```mermaid
flowchart TB
    App["📱 Tu Aplicación"] --> API["🔌 API del Proveedor IA"]

    API --> Patrones["📋 Patrones de Integración"]

    Patrones --> Streaming["📡 Streaming<br/>Respuesta en tiempo real"]
    Patrones --> Structured["📊 Salidas Estructuradas<br/>JSON / esquemas definidos"]
    Patrones --> FunctionCalling["⚙️ Function Calling<br/>El modelo llama herramientas"]
    Patrones --> Batch["📦 Batch<br/>Múltiples requests en lote"]
    Patrones --> Embed["🔢 Embeddings<br/>Vectores para búsqueda"]

    style App fill:#e1f5fe
    style API fill:#ffcc80
    style Patrones fill:#e8f5e9
    style Streaming fill:#fff3e0
    style Structured fill:#c8e6c9
    style FunctionCalling fill:#fce4ec
    style Batch fill:#e8eaf6
    style Embed fill:#f3e5f5
```

---

## Streaming

El **streaming** permite que el modelo envíe la respuesta **token por token** a medida que los genera, en lugar de esperar a tener la respuesta completa.

```mermaid
sequenceDiagram
    participant User as 👤 Usuario
    participant App as 📱 App
    participant API as 🔌 API Proveedor
    participant Model as 🧠 Modelo IA

    User->>App: "Escribe un cuento"
    App->>API: POST /chat (stream: true)

    Note over App,Model: La conexión se mantiene abierta

    Model-->>API: Token 1: "Érase"
    API-->>App: "Érase"
    App-->>User: "Érase"

    Model-->>API: Token 2: " una"
    API-->>App: " una"
    App-->>User: " una"

    Model-->>API: Token 3: " vez"
    API-->>App: " vez"
    App-->>User: " vez"

    Model-->>API: Token 4: "..."
    API-->>App: "..."
    App-->>User: "..."

    Note over User,Model: El usuario ve el texto aparecer<br/>en tiempo real (como ChatGPT)

    API-->>App: [DONE]
    App-->>User: ✅ Cuento completo
```

### Sin streaming vs Con streaming

```mermaid
flowchart LR
    subgraph SinStreaming ["🚫 Sin Streaming"]
        N1["Usuario envía prompt"] --> N2["Espera 5-10 segundos"]
        N2 --> N3["Recibe respuesta completa<br/>de golpe"]
        N3 --> N4["😐 Experiencia: lenta,<br/>no sabe si funciona"]
    end

    subgraph ConStreaming ["✅ Con Streaming"]
        S1["Usuario envía prompt"] --> S2["Empieza a recibir<br/>texto en < 1 segundo"]
        S2 --> S3["Ve el texto generarse<br/>en tiempo real"]
        S3 --> S4["😊 Experiencia: rápida,<br/>siente que 'piensa'"]
    end

    style SinStreaming fill:#ffcdd2
    style ConStreaming fill:#c8e6c9
```

### Implementación

```javascript
// Node.js con OpenAI (streaming)
const response = await openai.chat.completions.create({
    model: "gpt-4o",
    messages: [{ role: "user", content: "Escribe un cuento" }],
    stream: true,  // ← activa streaming
});

for await (const chunk of response) {
    const token = chunk.choices[0]?.delta?.content || "";
    process.stdout.write(token);  // muestra token en tiempo real
}
```

```python
# Python con OpenAI (streaming)
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Escribe un cuento"}],
    stream=True
)

for chunk in response:
    token = chunk.choices[0].delta.content or ""
    print(token, end="", flush=True)
```

| Ventajas                       | Desventajas                          |
| ------------------------------ | ------------------------------------ |
| ✅ Mejor experiencia de usuario | ⚠️ Más complejo de implementar       |
| ✅ Feedback inmediato           | ⚠️ No puedes reutilizar la respuesta fácilmente |
| ✅ Percibido como más rápido    | ⚠️ Manejo de interrupciones          |

---

## Salidas estructuradas

Las **salidas estructuradas** garantizan que el modelo devuelva datos en un **formato específico** (JSON, XML, etc.) que tu aplicación pueda parsear de forma confiable.

```mermaid
flowchart LR
    subgraph SinEstructura["🚫 Sin estructura"]
        A1["Prompt: Dame los datos<br/>del usuario"]
        A2["Respuesta:<br/>Ana, 25 años,<br/>ana@email.com"]
        A3["❌ Difícil de procesar<br/>con código"]

        A1 --> A2 --> A3
    end

    subgraph ConEstructura["✅ Con estructura"]
        B1["Prompt: Dame los datos<br/>del usuario en JSON"]
        B2["Respuesta JSON<br/>nombre: Ana<br/>edad: 25<br/>email: ana@email.com"]
        B3["✅ Fácil de parsear<br/>con JSON.parse()"]

        B1 --> B2 --> B3
    end

    style SinEstructura fill:#ffcdd2
    style ConEstructura fill:#c8e6c9
```

### Con schema definido (recomendado)

Los proveedores modernos permiten definir un **schema JSON** que el modelo debe respetar estrictamente.

```python
# Python con OpenAI (structured outputs)
from pydantic import BaseModel

class Usuario(BaseModel):
    nombre: str
    edad: int
    email: str
    activo: bool

response = client.beta.chat.completions.parse(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Ana, 25 años, anA@email.com"}],
    response_format=Usuario,  # ← esquema definido
)

usuario = response.choices[0].message.parsed
print(usuario.nombre)  # → "Ana"
```

```javascript
// Node.js con OpenAI (structured outputs)
const response = await openai.chat.completions.create({
    model: "gpt-4o",
    messages: [{ role: "user", content: "Ana, 25 años" }],
    response_format: {
        type: "json_schema",
        json_schema: {
            name: "usuario",
            schema: {
                type: "object",
                properties: {
                    nombre: { type: "string" },
                    edad: { type: "number" },
                    email: { type: "string" }
                },
                required: ["nombre", "edad", "email"]
            }
        }
    }
});
```

### Casos de uso

| Caso                    | Esquema de salida                     |
| ----------------------- | ------------------------------------- |
| **Extraer datos**       | JSON con campos específicos           |
| **Clasificación**       | `{ "categoria": "spam" }`            |
| **Generar UI**          | `{ "type": "button", "text": "..." }` |
| **Agentes**             | `{ "tool": "search", "args": {...} }` |

> **Beneficio clave:** Eliminas la necesidad de parsear texto libre. El modelo devuelve datos que tu código puede consumir directamente.

---

## Function Calling

El **function calling** (o tool use) permite que el modelo **decida llamar funciones de tu código** cuando sea necesario. Es la base de los agentes.

```mermaid
sequenceDiagram
    participant User as 👤 Usuario
    participant App as 📱 App
    participant Model as 🧠 Modelo
    participant Func as ⚙️ Tus Funciones
    participant API as 🌍 API Externa

    User->>App: "¿Qué clima hace en Madrid?"
    App->>Model: Prompt + definiciones de funciones

    Note over App,Model: Defino: get_weather(city)

    Model->>App: 🤔 Necesito llamar get_weather("Madrid")
    App->>Func: Ejecuta get_weather("Madrid")
    Func->>API: GET api.clima.com/madrid
    API-->>Func: 🌡️ 25°C
    Func-->>App: { temperature: 25, condition: "soleado" }

    App->>Model: Envío resultado de la función
    Model->>App: "En Madrid hace 25°C y está soleado ☀️"
    App-->>User: "En Madrid hace 25°C y está soleado ☀️"
```

### Definición de funciones

```javascript
// Node.js: definir funciones disponibles
const tools = [
    {
        type: "function",
        function: {
            name: "get_weather",
            description: "Obtener el clima de una ciudad",
            parameters: {
                type: "object",
                properties: {
                    city: {
                        type: "string",
                        description: "Nombre de la ciudad"
                    }
                },
                required: ["city"]
            }
        }
    }
];

// Enviar al modelo
const response = await openai.chat.completions.create({
    model: "gpt-4o",
    messages: [{ role: "user", content: "¿Clima en Madrid?" }],
    tools: tools  // ← el modelo puede llamarlas
});

// El modelo pide llamar la función
const toolCall = response.choices[0].message.tool_calls[0];
// → { function: { name: "get_weather", arguments: '{"city":"Madrid"}' } }
```

```python
# Python con OpenAI
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Obtener el clima de una ciudad",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {"type": "string"}
                },
                "required": ["city"]
            }
        }
    }
]

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "¿Clima en Madrid?"}],
    tools=tools
)
```

### Ciclo completo de function calling

```mermaid
flowchart TB
    Start["💬 Prompt del usuario"] --> Model1["🧠 Modelo decide:<br/>¿Necesito herramienta?"]

    Model1 -->|"✅ Sí"| Call["📞 Llama función con argumentos"]
    Model1 -->|"❌ No"| Direct["📤 Responde directamente"]

    Call --> Execute["⚙️ Ejecutas la función<br/>(tu código)"]
    Execute --> Result["📦 Resultado de la función"]
    Result --> Model2["🧠 Modelo procesa resultado"]
    Model2 --> Response["📤 Respuesta final al usuario"]

    style Start fill:#e1f5fe
    style Model1 fill:#fff3e0
    style Call fill:#fce4ec
    style Execute fill:#c8e6c9
    style Result fill:#e8eaf6
    style Model2 fill:#fff3e0
    style Response fill:#c8e6c9
```

### Buenas prácticas

- **Descripciones claras** — explica qué hace cada función y cada parámetro
- **Errores manejados** — si la función falla, devuelve un error que el modelo entienda
- **No confíes en el input** — valida los argumentos que el modelo envía
- **Funciones atómicas** — cada función hace una cosa bien

---

## Comparativa de patrones

```mermaid
flowchart TB
    Pregunta{"🔍 ¿Qué necesitas?"}

    Pregunta -->|"Respuesta en tiempo real"| Stream["📡 Streaming<br/>Chat, asistentes"]
    Pregunta -->|"Datos parseables"| Struct["📊 Salidas Estructuradas<br/>Extracción, clasificación"]
    Pregunta -->|"El modelo actúa"| FC["⚙️ Function Calling<br/>Agentes, herramientas"]
    Pregunta -->|"Procesar muchos datos"| Batch2["📦 Batch<br/>Análisis masivo"]
    Pregunta -->|"Búsqueda semántica"| Embed2["🔢 Embeddings<br/>RAG, recomendación"]

    style Pregunta fill:#e1f5fe
    style Stream fill:#fff3e0
    style Struct fill:#c8e6c9
    style FC fill:#fce4ec
    style Batch2 fill:#e8eaf6
    style Embed2 fill:#f3e5f5
```

---

## Ejemplo completo: asistente del clima

```mermaid
flowchart TB
    User["👤 '¿Lloverá mañana<br/>en Barcelona?'"]

    User --> Stream1["📡 Streaming ON<br/>El usuario ve la respuesta<br/>mientras se genera"]

    User --> Func1["⚙️ El modelo llama:<br/>get_weather('Barcelona')"]
    Func1 --> Func2["⚙️ get_forecast('Barcelona')"]

    Func2 --> Struct2["📊 Resultados estructurados"]
    Struct2 --> JSON["{ temperature: 22,<br/>precipitation: 0.8,<br/>condition: 'rain' }"]

    JSON --> Final["🤖 'Sí, mañana lloverá<br/>en Barcelona. 🌧️<br/>Temp: 22°C'"]
    Final --> User

    style User fill:#e1f5fe
    style Stream1 fill:#fff3e0
    style Func1 fill:#fce4ec
    style Func2 fill:#fce4ec
    style Struct2 fill:#c8e6c9
    style JSON fill:#e8eaf6
    style Final fill:#e8f5e9
```

---

## Resumen visual

```mermaid
graph TB
    Patrones2["📋 Patrones de Integración"] --> Streaming2["📡 Streaming<br/>Tokens en tiempo real"]
    Patrones2 --> Structured2["📊 Salidas Estructuradas<br/>JSON con schema"]
    Patrones2 --> FC2["⚙️ Function Calling<br/>Modelo llama herramientas"]
    Patrones2 --> Batch3["📦 Batch<br/>Múltiples requests"]
    Patrones2 --> Embed3["🔢 Embeddings<br/>Vectores para búsqueda"]

    Streaming2 --> Chat["💬 Chats, asistentes"]
    Structured2 --> Extraccion["📄 Extracción de datos"]
    FC2 --> Agentes["🤖 Agentes autónomos"]
    Batch3 --> Analisis["📈 Análisis masivo"]
    Embed3 --> RAG["🔍 RAG, búsqueda"]

    style Patrones2 fill:#e1f5fe
    style Streaming2 fill:#fff3e0
    style Structured2 fill:#c8e6c9
    style FC2 fill:#fce4ec
    style Batch3 fill:#e8eaf6
    style Embed3 fill:#f3e5f5
```

> **Siguiente paso:** Implementa streaming en tu app para mejor UX, agrega salidas estructuradas para integrar IA con tu lógica de negocio, y experimenta con function calling para crear tu primer agente autónomo.

## Relacionados:
- [[proveedores-de-ia]] #anterior 
- [[ci-cd]] #siguiente 