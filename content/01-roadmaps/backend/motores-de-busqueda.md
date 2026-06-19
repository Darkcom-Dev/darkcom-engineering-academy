# Motores de búsqueda

Los **motores de búsqueda** (search engines) están diseñados para **indexar y buscar grandes volúmenes de texto** de forma rápida y relevante. No son bases de datos (aunque almacenan datos): su fuerte es la **búsqueda full-text**, los **filtros** y la **relevancia**.

```mermaid
flowchart LR
    User["👤 Usuario"] -->|"Busca: 'laptop gaming'"| App["📱 App"]
    App -->|"Query"| Search["🔍 Motor de Búsqueda"]
    Search -->|"Resultados relevantes"| App
    App --> User

    BD["🗄️ BD Relacional"] -.->|"Sincroniza datos"| Search

    style User fill:#e1f5fe
    style App fill:#fff3e0
    style Search fill:#c8e6c9
    style BD fill:#fce4ec
```

### BD vs Motor de Búsqueda

| Característica       | Base de Datos (PostgreSQL) | Motor de Búsqueda (ES) |
| -------------------- | -------------------------- | ---------------------- |
| **Búsqueda de texto**| LIKE / ILIKE (lento)       | Full-text indexado     |
| **Relevancia**       | ❌ (exacto o nada)         | ✅ Scoring por relevancia |
| **Fuzzy search**     | ❌                         | ✅ (errores ortográficos) |
| **Facetas**          | ❌ (GROUP BY complejo)     | ✅ Aggregations        |
| **Tiempo real**      | Depende del índice         | Casi tiempo real       |
| **Analítica**        | Agregaciones simples       | Pipeline de agregactions |

### ¿Cuándo usar un motor de búsqueda?

```mermaid
flowchart TD
    Pregunta{"🔍 ¿Necesitas..."}

    Pregunta -->|"Búsqueda full-text<br/>en millones de docs"| ES["✅ Usa Elasticsearch / Solr"]
    Pregunta -->|"Búsqueda con errores<br/>ortográficos (fuzzy)"| ES
    Pregunta -->|"Resultados ordenados<br/>por relevancia"| ES
    Pregunta -->|"Filtros y facetas<br/>(precio, categoría)"| ES
    Pregunta -->|"JOINs complejos,<br/>transacciones ACID"| BD2["✅ Usa BD Relacional"]

    style Pregunta fill:#e1f5fe
    style ES fill:#c8e6c9
    style BD2 fill:#e3f2fd
```

---

## Elasticsearch

**Elasticsearch** es el motor de búsqueda y analítica más usado del mundo. Es distribuido, escalable horizontalmente y está basado en **Apache Lucene**.

```mermaid
graph TB
    subgraph ElasticArch ["⚡ Arquitectura Elasticsearch"]
        direction LR

        subgraph Cluster ["☁️ Cluster"]
            Node1["🖥️ Nodo 1"]
            Node2["🖥️ Nodo 2"]
            Node3["🖥️ Nodo 3"]
        end

        App2["📱 App"] --> Cluster

        Cluster --> Index1["📂 Índice: productos<br/>│<br/>└── Shard 1 (Nodo 1)"]
        Cluster --> Index2["📂 Índice: pedidos<br/>│<br/>├── Shard 1 (Nodo 2)<br/>└── Shard 2 (Nodo 3)"]

        Index1 --> Replica["🔄 Réplica: Shard 1 → Nodo 2"]
    end

    style ElasticArch fill:#f5f5f5
    style Cluster fill:#e3f2fd
    style App2 fill:#e1f5fe
    style Index1 fill:#fff3e0
    style Index2 fill:#fce4ec
    style Replica fill:#c8e6c9
```

### Conceptos clave

```mermaid
flowchart TB
    ES2["🔍 Elasticsearch"] --> Document["📄 Documento<br/>Unidad de datos (JSON)"]
    ES2 --> Index["📂 Índice<br/>Colección de documentos<br/>(como una tabla en BD)"]
    ES2 --> Shard["🧩 Shard<br/>Subdivisión del índice<br/>(Paralelismo + Escalado)"]
    ES2 --> Node["🖥️ Nodo<br/>Servidor individual"]
    ES2 --> Cluster2["☁️ Cluster<br/>Conjunto de nodos"]
    ES2 --> Mapping["📋 Mapping<br/>Schema del documento<br/>(tipos de campos)"]
    ES2 --> Inverted["📖 Índice invertido<br/>Palabra → Documentos<br/>(búsqueda instantánea)"]

    style ES2 fill:#e1f5fe
    style Document fill:#fff3e0
    style Index fill:#c8e6c9
    style Shard fill:#fce4ec
    style Node fill:#e8eaf6
    style Cluster2 fill:#f3e5f5
    style Mapping fill:#fff9c4
    style Inverted fill:#e8f5e9
```

### ¿Qué es el índice invertido?

```mermaid
flowchart LR
    subgraph Documentos ["📄 Documentos"]
        D1["Doc 1: 'El gato duerme'"]
        D2["Doc 2: 'El perro corre'"]
        D3["Doc 3: 'Gato y perro juegan'"]
    end

    subgraph InvertedIndex ["📖 Índice Invertido"]
        I1["el → Doc 1, Doc 2"]
        I2["gato → Doc 1, Doc 3"]
        I3["perro → Doc 2, Doc 3"]
        I4["duerme → Doc 1"]
        I5["corre → Doc 2"]
        I6["juegan → Doc 3"]
    end

    Documentos --> InvertedIndex
    Query["🔍 'gato'"] --> I2
    I2 --> Result["✅ Doc 1, Doc 3"]

    style Documentos fill:#fff3e0
    style InvertedIndex fill:#c8e6c9
    style Query fill:#e1f5fe
    style Result fill:#e8f5e9
```

> **Analogía:** El índice invertido es como el índice al final de un libro: tú buscas "gato" y te dice en qué páginas (documentos) aparece.

### Operaciones básicas

```bash
# Indexar un documento (crear/actualizar)
PUT /productos/_doc/1
{
    "nombre": "Laptop Gamer",
    "precio": 1500,
    "categoria": "electrónica",
    "descripcion": "Laptop con RTX 4070, 32GB RAM"
}

# Buscar
GET /productos/_search
{
    "query": {
        "match": {
            "descripcion": "laptop gaming"
        }
    }
}
```

### Tipos de consulta

```json
// Búsqueda full-text con relevancia
{
    "query": {
        "match": { "descripcion": "laptop gaming" }
    }
}

// Búsqueda exacta (filtro)
{
    "query": {
        "term": { "categoria": "electrónica" }
    }
}

// Combinación (bool)
{
    "query": {
        "bool": {
            "must": [{ "match": { "descripcion": "laptop" } }],
            "filter": [{ "term": { "categoria": "electrónica" } }],
            "must_not": [{ "term": { "marca": "lenovo" } }]
        }
    }
}

// Fuzzy (tolera errores)
{
    "query": {
        "fuzzy": {
            "nombre": {
                "value": "laptop",
                "fuzziness": "AUTO"
            }
        }
    }
}

// Agregaciones (facetas)
{
    "size": 0,
    "aggs": {
        "por_categoria": {
            "terms": { "field": "categoria" }
        },
        "precio_promedio": {
            "avg": { "field": "precio" }
        }
    }
}
```

### Stack ELK

Elasticsearch suele usarse con **Kibana** (visualización) y **Logstash** (ingesta de datos) → el stack **ELK**.

```mermaid
flowchart LR
    Logs["📋 Logs de servidores"] --> Logstash["📥 Logstash<br/>(procesa y envía)"]
    Apps["📱 Apps"] --> Logstash
    BD["🗄️ BD"] --> Logstash

    Logstash --> ES3["🔍 Elasticsearch<br/>(almacena e indexa)"]
    ES3 --> Kibana["📊 Kibana<br/>(visualiza y consulta)"]
    Kibana --> User2["👤 Usuario"]

    style Logs fill:#fce4ec
    style Apps fill:#e1f5fe
    style Logstash fill:#ffe0b2
    style ES3 fill:#c8e6c9
    style Kibana fill:#e3f2fd
    style User2 fill:#e8f5e9
```

---

## Solr

**Apache Solr** también está basado en Apache Lucene (como Elasticsearch). Fue el primer motor de búsqueda empresarial open source y aún se usa en aplicaciones legacy y sistemas donde importa más la madurez que la velocidad de evolución.

```mermaid
graph TB
    subgraph SolrFeatures ["☀️ Apache Solr"]
        Lucene["🔤 Mismo Lucene<br/>que Elasticsearch"]
        Config["📋 Configuración<br/>XML (solrconfig.xml)"]
        Schema["📐 Schema<br/>XML / JSON / managed-schema"]
        Admin["🖥️ UI Admin<br/>Integrada"]
        Security["🔒 Seguridad<br/>Madura, enterprise"]
    end

    style SolrFeatures fill:#fff9c4
```

### Elasticsearch vs Solr

| Característica        | Elasticsearch         | Solr                 |
| --------------------- | --------------------- | -------------------- |
| **Configuración**     | JSON (API REST)       | XML (archivos)       |
| **Escalabilidad**     | Nativa (sharding automático) | Manual (requiere ZooKeeper) |
| **Tiempo real**       | Casi real (1s)        | Casi real            |
| **UI**                | Kibana (separado)     | Admin UI integrada   |
| **Ecosistema**        | ELK Stack (enorme)    | Limitado             |
| **Casos de uso**      | Búsqueda web, logs, analítica | Búsqueda empresarial, e-commerce |
| **Popularidad**       | ⭐⭐⭐⭐⭐             | ⭐⭐⭐               |

### ¿Cuál elegir?

```mermaid
flowchart TD
    P{"🔍 ¿Qué buscas?"}
    P -->|"Nuevo proyecto,<br/>escalabilidad, ecosistema"| ES4["✅ Elasticsearch<br/>Más popular, más features"]
    P -->|"Sistema legacy,<br/>madurez, enterprise"| Solr2["✅ Solr<br/>Estable, probado"]
    P -->|"Análisis de logs<br/>+ visualización"| ES4
    P -->|"Búsqueda en<br/>e-commerce complejo"| Solr2

    style P fill:#e1f5fe
    style ES4 fill:#c8e6c9
    style Solr2 fill:#fff9c4
```

---

## Integración con tu backend

### Patrón típico: BD + Elasticsearch

```mermaid
sequenceDiagram
    participant App as 📱 App
    participant BD as 🗄️ PostgreSQL
    participant ES as 🔍 Elasticsearch
    participant User as 👤 Usuario

    Note over App,ES: ESCRITURA: siempre va a BD primero

    App->>BD: INSERT producto
    BD-->>App: ✅ Guardado

    App->>ES: Indexar producto en ES
    Note right of ES: Sincronización asíncrona

    User->>App: Buscar "laptop"
    App->>ES: Buscar en ES (no en BD)
    ES-->>App: Resultados relevantes
    App-->>User: Lista de productos
```

### Sincronización BD → ES

```javascript
// Después de guardar en BD, indexar en ES
async function crearProducto(data) {
    // 1. Guardar en BD
    const producto = await db.query(
        'INSERT INTO productos SET ?', [data]
    );

    // 2. Indexar en Elasticsearch
    await esClient.index({
        index: 'productos',
        id: producto.insertId,
        body: data
    });

    return producto;
}

// Búsqueda
async function buscarProductos(query) {
    const result = await esClient.search({
        index: 'productos',
        body: {
            query: {
                match: { nombre: query }
            }
        }
    });

    return result.hits.hits.map(h => h._source);
}
```

---

## Buenas prácticas

- **No uses ES como BD primaria** — los datos van en BD, ES es para búsqueda
- **Mapea campos correctamente** — `text` para búsqueda full-text, `keyword` para filtros exactos
- **Normaliza texto** — lowercase, stemming (raíces), stop words
- **Alias de índices** — usa alias para hacer reindexación sin downtime
- **Monitoreo** — Heap, índices, shards (Kibana Stack Monitoring)
- **Sharding adecuado** — ni muy pocos (1-2) ni demasiados shards
- **Refresh interval** — ajusta según necesidad de tiempo real vs rendimiento

---

## Resumen visual

```mermaid
graph TB
    SE["🔍 Motores de Búsqueda"]

    SE --> Elastic["⚡ Elasticsearch<br/>Búsqueda moderna<br/>API REST JSON<br/>Escalable, tiempo real<br/>Stack ELK"]

    SE --> Solr3["☀️ Apache Solr<br/>Búsqueda empresarial<br/>Config XML<br/>Maduro, estable"]

    SE --> Cloud["☁️ Gestionados"]
    Cloud --> ElasticCloud["Elastic Cloud"]
    Cloud --> Algolia["Algolia (SaaS)"]
    Cloud --> Meili["Meilisearch (Open Source)"]

    SE --> Conceptos["📖 Índice invertido<br/>Shards + Réplicas<br/>Relevancia (scoring)"]

    style SE fill:#e1f5fe
    style Elastic fill:#c8e6c9
    style Solr3 fill:#fff9c4
    style Cloud fill:#f5f5f5
    style Conceptos fill:#e8eaf6
```

> **Siguiente paso:** Levanta Elasticsearch con Docker, indexa unos documentos de prueba y juega con las queries match, term, bool y fuzzy. Luego conéctalo desde tu backend.

## Relacionados:
- [[broker-de-mensajes]] #anterior 
- [[patrones-arquitectonicos]] #siguiente 