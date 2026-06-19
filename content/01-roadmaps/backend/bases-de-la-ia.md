# Bases de inteligencia artificial

La **Inteligencia Artificial** (IA) es la rama de la computación que crea sistemas capaces de realizar tareas que normalmente requieren inteligencia humana: entender lenguaje, reconocer imágenes, tomar decisiones.

```mermaid
graph TB
    AI["🧠 Inteligencia Artificial"] --> ML["📊 Machine Learning<br/>(aprende de datos)"]
    AI --> RuleBased["📋 Sistemas Basados en Reglas<br/>(if-else tradicional)"]

    ML --> DeepLearning["🔬 Deep Learning<br/>(redes neuronales)"]
    ML --> ClassicML["📈 ML Clásico<br/>(regresión, árboles, SVM)"]

    DeepLearning --> LLM["📝 LLM - Modelos de Lenguaje<br/>(GPT, Claude, Llama)"]
    DeepLearning --> Vision["👁️ Visión por Computadora"]
    DeepLearning --> Audio["🎤 Reconocimiento de Voz"]

    style AI fill:#e1f5fe
    style ML fill:#c8e6c9
    style DeepLearning fill:#e3f2fd
    style LLM fill:#fff3e0
```

---

## ¿Cómo funcionan los LLM?

Un **LLM** (Large Language Model) es una red neuronal entrenada con **enormes cantidades de texto** para predecir la siguiente palabra en una secuencia. Así de simple en esencia: **predecir la siguiente palabra**.

```mermaid
flowchart LR
    subgraph Entrenamiento ["🏋️ Entrenamiento"]
        Data["📚 Billones de palabras<br/>(libros, web, artículos)"] --> Train["🧠 Red Neuronal<br/>(Transformer)"]
        Train --> Weights["⚖️ Pesos del modelo<br/>(~billones de parámetros)"]
    end

    subgraph Inferencia ["⚡ Inferencia (uso)"]
        Prompt["💬 Input: 'El gato está sobre la'"] --> Model["🧠 Modelo"]
        Model --> Predict["🔮 Predice: 'alfombra'"]
        Predict --> Context["📎 Añade al contexto<br/>y predice la siguiente..."]
    end

    style Entrenamiento fill:#e8f5e9
    style Inferencia fill:#fff3e0
    style Data fill:#bbdefb
    style Weights fill:#c8e6c9
    style Prompt fill:#ffe0b2
    style Predict fill:#ffcc80
```

### La arquitectura Transformer

Los LLM modernos usan la arquitectura **Transformer** (Google, 2017). Su innovación clave es el **mecanismo de atención** (self-attention).

```mermaid
flowchart TB
    Input["📥 Input: El gato está en la"] --> Embed["🔢 Embeddings<br/>(palabras → vectores)"]
    Embed --> Attn["🧠 Self-Attention<br/>(¿qué palabras son relevantes?)"]
    Attn --> FF["⚙️ Feed-Forward<br/>(procesa la información)"]
    FF --> Output["📤 Output: alfombra<br/>(probabilidad sobre todas las palabras)"]

    subgraph Attention["Cómo funciona la atención"]
        A1["'gato' se relaciona con 'está'"]
        A2["'gato' se relaciona con 'la'"]
        A3["Cada palabra se relaciona con todas las demás"]
    end

    style Input fill:#e1f5fe
    style Embed fill:#fce4ec
    style Attn fill:#fff9c4
    style FF fill:#e8f5e9
    style Output fill:#c8e6c9
```

### Conceptos clave de LLMs

| Concepto           | Explicación                                      |
| ------------------ | ------------------------------------------------ |
| **Parámetros**     | Pesos del modelo (~billones). Más parámetros ≠ mejor, pero suele correlacionar |
| **Contexto**       | Ventana de texto que el modelo puede "ver" (ej: 8K, 32K, 128K tokens) |
| **Tokens**         | Fragmentos de palabras (~0.75 palabras en inglés). El modelo lee/ genera tokens |
| **Temperature**    | Controla cuán "creativa" es la respuesta (0 = siempre la misma, 1 = más variada) |
| **Top-p / Top-k**  | Estrategias para elegir la siguiente palabra entre las más probables |
| **Fine-tuning**    | Entrenar el modelo con datos específicos para una tarea concreta |

> **Analogía:** Un LLM es como un pianista que ha escucho millones de canciones. No "entiende" la música, pero sabe qué nota suele ir después de otra.

---

## IA vs Codificación tradicional

No son competencia, son herramientas para problemas diferentes.

```mermaid
flowchart TB
    Pregunta{"📋 ¿El problema tiene<br/>reglas definidas?"}

    Pregunta -->|"✅ Sí, reglas exactas"| Tradicional["💻 Código Tradicional<br/>if-else, loops, algoritmos"]
    Tradicional --> EjemT["Sumar dos números<br/>Validar un email<br/>Ordenar una lista"]

    Pregunta -->|"❌ No, hay patrones<br/>pero no reglas"| IA["🤖 IA / ML"]
    IA --> EjemI["Reconocer una foto de un gato<br/>Traducir un idioma<br/>Detectar spam en emails"]

    Tradicional --> Certeza["✅ Determinista<br/>Misma entrada = misma salida"]
    IA --> Probabilidad["🎲 Probabilístico<br/>Misma entrada = puede variar"]

    style Pregunta fill:#e1f5fe
    style Tradicional fill:#c8e6c9
    style IA fill:#fff3e0
    style EjemT fill:#c8e6c9
    style EjemI fill:#fff3e0
    style Certeza fill:#e8f5e9
    style Probabilidad fill:#fff9c4
```

### Tabla comparativa

| Característica       | Código tradicional               | IA / Machine Learning             |
| -------------------- | -------------------------------- | --------------------------------- |
| **Reglas**           | Las escribes tú (if-else)        | Las aprende de datos              |
| **Precisión**        | 100% (si está bien programado)   | >90% (nunca perfecto)            |
| **Explicabilidad**   | Total (sabes exactamente qué hace) | Baja (caja negra)               |
| **Escalabilidad**    | Necesitas programar cada caso    | Con más datos, mejora solo        |
| **Mantenimiento**    | Actualizas el código             | Actualizas los datos y reentrenas |

### ¿Cuándo usar cada uno?

```javascript
// ✅ Código tradicional: validar formato de email
function esEmailValido(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

// ❌ NO usarías IA para esto (sería como usar un cañón para matar una mosca)
```

```python
# ✅ IA: detectar si un email es phishing
# No puedes escribir reglas exactas para detectar todos los casos de phishing
model.predict("Ganaste $1,000,000!!! Haz click aquí")
# → 95% probabilidad de phishing
```

---

## Embeddings

Un **embedding** es una **representación vectorial** de un texto (palabra, frase, documento) en un espacio de N dimensiones, donde palabras con significado similar quedan **cerca** entre sí.

```mermaid
flowchart LR
    subgraph EmbeddingProcess ["🔢 Palabras → Vectores"]
        Word1["'gato'"] --> Vec1["[0.2, -0.5, 0.8, 0.1, -0.3, ...]"]
        Word2["'felino'"] --> Vec2["[0.21, -0.48, 0.79, 0.12, -0.31, ...]"]
        Word3["'computadora'"] --> Vec3["[-0.7, 0.3, -0.1, 0.9, 0.4, ...]"]
    end

    subgraph Space ["🌌 Espacio vectorial"]
        V1["gato 🐱"]
        V2["felino 🐱"]
        V3["perro 🐕"]
        V4["computadora 💻"]
        V5["teléfono 📱"]
    end

    V1 --- V2
    V1 --- V3
    V4 --- V5

    style EmbeddingProcess fill:#fce4ec
    style Space fill:#e8f5e9
```

> **Analogía:** Los embeddings son como coordenadas GPS del lenguaje. "Madrid" y "Barcelona" están cerca. "Madrid" y "manzana" están lejos.

### ¿Para qué sirven?

- **Búsqueda semántica** — encontrar documentos por significado, no por palabras exactas
- **Clasificación** — agrupar textos similares
- **Recomendación** — encontrar contenido relacionado
- **RAG** — recuperar información relevante para un LLM

```python
# Ejemplo con OpenAI
response = openai.embeddings.create(
    model="text-embedding-3-small",
    input="El gato está sobre la alfombra"
)
vector = response.data[0].embedding  # → [0.002, -0.015, ... 1536 dimensiones]
```

---

## Vectores

Los vectores son la **representación matemática** detrás de embeddings y búsqueda por similitud.

```mermaid
graph LR
    subgraph VectorSpace ["🌌 Espacio vectorial 2D"]
        A["📄 Doc1: 'Receta de paella'<br/>(0.9, 0.2)"]
        B["📄 Doc2: 'Cocina española'<br/>(0.85, 0.25)"]
        C["📄 Doc3: 'Física cuántica'<br/>(-0.8, 0.6)"]
    end

    A -.- B
    A -.- C

    Query["🔍 Query: 'comida valenciana'<br/>(0.88, 0.22)"] --> Similar["✅ Doc1 y Doc2 son los<br/>más cercanos (similitud)"]

    style VectorSpace fill:#e8f5e9
    style Query fill:#fff3e0
    style Similar fill:#c8e6c9
```

### Búsqueda por similitud (coseno)

Para encontrar textos similares, se calcula la **distancia coseno** entre vectores. Cuanto más cerca de 1, más similares.

```python
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity

vector_paella = [0.9, 0.2]
vector_cocina = [0.85, 0.25]
vector_fisica = [-0.8, 0.6]
vector_query = [0.88, 0.22]

sim_paella = cosine_similarity([vector_query], [vector_paella])  # → ~0.99
sim_fisica = cosine_similarity([vector_query], [vector_fisica])  # → ~-0.5
```

### Bases de datos vectoriales

Para buscar entre millones de vectores de forma eficiente, se usan **bases de datos vectoriales**:

| Base de datos     | Tipo                      |
| ----------------- | ------------------------- |
| **Pinecone**      | SaaS (gestionado)         |
| **Weaviate**      | Open source + cloud       |
| **Qdrant**        | Open source + cloud       |
| **ChromaDB**      | Open source, embebida     |
| **pgvector**      | Extensión para PostgreSQL |

```sql
-- pgvector: buscar los 5 documentos más similares
SELECT * FROM documentos
ORDER BY embedding <=> '[0.88, 0.22, ...]'
LIMIT 5;
```

---

## RAGs (Retrieval-Augmented Generation)

**RAG** es una técnica que combina **búsqueda en una base de conocimiento** con la **generación de un LLM**. Resuelve el problema principal de los LLM: no tienen acceso a tu información privada o actual.

```mermaid
flowchart TB
    User["👤 Usuario pregunta:<br/>'¿Cuál es la política de<br/>devoluciones?'"] --> Query["🔍 Embedding de la pregunta"]

    Query --> VectorDB["🗄️ Base de Datos<br/>Vectorial"]
    VectorDB --> Context["📄 Documentos relevantes<br/>(política de devoluciones)"]

    Context --> Prompt["📝 Prompt con contexto:<br/>'Usando estos documentos:<br/>...responde la pregunta'"]
    User --> Prompt
    Prompt --> LLM["🧠 LLM"]
    LLM --> Response["💬 Respuesta basada<br/>en TU información"]

    style User fill:#e1f5fe
    style Query fill:#fce4ec
    style VectorDB fill:#c8e6c9
    style Context fill:#fff3e0
    style Prompt fill:#e8eaf6
    style LLM fill:#e8f5e9
    style Response fill:#ffcc80
```

### Sin RAG vs Con RAG

```mermaid
flowchart LR
    subgraph SinRAG ["🚫 Solo LLM"]
        Q1["¿Cuál es el horario<br/>de atención? "] --> LLM1["🧠 LLM<br/>(conocimiento general)"]
        LLM1 --> R1["No lo sé, no tengo<br/>acceso a esa información"]
    end

    subgraph ConRAG ["✅ LLM + RAG"]
        Q2["¿Cuál es el horario<br/>de atención?"] --> Search2["🔍 Busca en BD<br/>vectorial"]
        Search2 --> Docs2["📄 Encuentra:<br/>'Horario: 9-18h'"]
        Docs2 --> LLM2["🧠 LLM + contexto"]
        LLM2 --> R2["Nuestro horario es<br/>de 9:00 a 18:00"]
    end

    style SinRAG fill:#ffcdd2
    style ConRAG fill:#c8e6c9
```

### Flujo típico de RAG

```python
# Pseudocódigo: RAG simplificado

# 1. Indexar documentos (una vez)
documentos = [
    "Política de devoluciones: 30 días...",
    "Horario: lunes a viernes 9-18h..."
]
for doc in documentos:
    vector = embedding_model.embed(doc)
    vector_db.insert(vector, doc)

# 2. Responder pregunta
def preguntar(pregunta_usuario):
    # Buscar documentos relevantes
    query_vector = embedding_model.embed(pregunta_usuario)
    resultados = vector_db.search(query_vector, top_k=3)

    # Construir prompt con contexto
    contexto = "\n".join(resultados)
    prompt = f"""
    Basado en esta información:
    {contexto}

    Responde: {pregunta_usuario}
    """

    # Generar respuesta
    return llm.generate(prompt)
```

### Componentes de un sistema RAG

```mermaid
flowchart TB
    RAG["🔧 Sistema RAG"] --> Indexing["📥 Indexación"]
    RAG --> Retrieval["🔍 Recuperación"]
    RAG --> Generation["✍️ Generación"]

    Indexing --> Chunk["📄 Dividir documentos<br/>en fragmentos (chunks)"]
    Indexing --> Embed["🔢 Generar embeddings"]
    Indexing --> Store["🗄️ Guardar en BD vectorial"]

    Retrieval --> QueryEmb["🔢 Embedding de la pregunta"]
    Retrieval --> Search["🔍 Búsqueda de similitud"]
    Retrieval --> Rank["📊 Ranking de resultados"]

    Generation --> Prompt2["📝 Prompt con contexto"]
    Generation --> LLM2["🧠 LLM genera respuesta"]

    style RAG fill:#e1f5fe
    style Indexing fill:#c8e6c9
    style Retrieval fill:#fff3e0
    style Generation fill:#e8eaf6
```

### Ventajas de RAG

| Ventaja               | Explicación                                      |
| --------------------- | ------------------------------------------------ |
| **Información actualizada** | La BD se actualiza sin reentrenar el modelo |
| **Datos privados**    | El LLM nunca ve tus datos (se quedan en tu BD)   |
| **Fuentes citables**  | Puedes mostrar al usuario de qué documento salió la respuesta |
| **Costo reducido**    | No necesitas fine-tunear el modelo               |
| **Alucinaciones**     | Se reducen drásticamente (el modelo responde sobre hechos, no de memoria) |

> **RAG es el patrón más usado hoy en día** para construir aplicaciones con IA que trabajen sobre información privada o actualizada. Es el motor detrás de chatbots de soporte, asistentes legales, buscadores semánticos, etc.

---

## Resumen visual

```mermaid
graph TB
    AI2["🤖 IA Aplicada"] --> LLM2["🧠 LLMs<br/>GPT, Claude, Llama"]
    AI2 --> Embeddings2["🔢 Embeddings<br/>Palabras → Vectores"]
    AI2 --> RAG2["🔧 RAG<br/>Búsqueda + Generación"]

    LLM2 --> Attencion["🎯 Mecanismo de Atención<br/>(Transformer)"]
    LLM2 --> Tokens["🔤 Tokens<br/>(~0.75 palabras)"]
    LLM2 --> Temperature["🌡️ Temperature<br/>(creatividad 0-1)"]

    Embeddings2 --> Similitud["📐 Similitud coseno"]
    Embeddings2 --> VectorDB2["🗄️ BD Vectoriales<br/>Pinecone, pgvector, Qdrant"]

    RAG2 --> Index["📥 Indexar documentos"]
    RAG2 --> Search2["🔍 Buscar relevantes"]
    RAG2 --> Generate["✍️ Generar respuesta"]

    AI2 --> Diff["⚡ IA vs Código Tradicional<br/>Patrones vs Reglas"]

    style AI2 fill:#e1f5fe
    style LLM2 fill:#fff3e0
    style Embeddings2 fill:#fce4ec
    style RAG2 fill:#e8f5e9
    style Diff fill:#e8eaf6
```

---

> **Siguiente paso:** Juega con la API de OpenAI o alguna open source (Ollama + Llama 3). Prueba a generar embeddings con `text-embedding-3-small` y construye tu primer RAG con LangChain o LlamaIndex.

## Relacionados:
- [[servidores-web]] #anterior 
- [[aplicaciones-con-ia]] #siguiente 