# Bases de datos NoSQL

Las bases de datos **NoSQL** (Not Only SQL) surgieron para manejar datos que no encajan bien en el modelo relacional: **gran volumen, esquemas flexibles, escalabilidad horizontal**. Cada tipo de NoSQL está optimizado para un caso de uso específico.

```mermaid
graph TB
    NoSQL["🗄️ Bases de Datos NoSQL"]

    NoSQL --> KeyValue["🔑 Key-Value<br/>Redis, DynamoDB"]
    NoSQL --> Document["📄 Documento<br/>MongoDB, CouchDB"]
    NoSQL --> Column["📊 Columnar<br/>Cassandra, ClickHouse"]
    NoSQL --> Graph["🕸️ Graph<br/>Neo4j, DGraph"]
    NoSQL --> TimeSeries["📈 Time Series<br/>InfluxDB, TimescaleDB"]
    NoSQL --> Realtime["⚡ Tiempo Real<br/>Firebase, RethinkDB"]

    style NoSQL fill:#e1f5fe
    style KeyValue fill:#fff3e0
    style Document fill:#c8e6c9
    style Column fill:#fce4ec
    style Graph fill:#e8eaf6
    style TimeSeries fill:#f3e5f5
    style Realtime fill:#fff9c4
```

### Cuándo usar NoSQL vs SQL

```mermaid
flowchart TD
    P{"🔍 ¿Tus datos tienen<br/>relaciones complejas?"}
    P -->|"Sí, necesitas JOINs<br/>y transacciones"| SQL["✅ SQL (PostgreSQL, MySQL)<br/>Relacional, ACID"]
    P -->|"No, son datos simples<br/>o jerárquicos"| NoSQL2["✅ NoSQL"]

    NoSQL2 --> Q2{"¿Qué tipo de datos?"}
    Q2 -->|"Documentos JSON<br/>flexibles"| Mongo["🍃 MongoDB"]
    Q2 -->|"Caché / sesiones<br/>/ contadores"| Redis2["🔴 Redis"]
    Q2 -->|"Alto throughput<br/>escrituras masivas"| Cass["🐘 Cassandra"]
    Q2 -->|"Grafos / relaciones<br/>sociales"| Neo["🔗 Neo4j"]
    Q2 -->|"Métricas / monitoreo<br/>series temporales"| Influx["📈 InfluxDB"]

    style P fill:#e1f5fe
    style SQL fill:#c8e6c9
    style NoSQL2 fill:#fff3e0
```

---

## Key-Value

Las bases de datos **clave-valor** son las más simples: guardan un valor asociado a una clave única. Son ultra rápidas, ideales para caché, sesiones y contadores.

```mermaid
flowchart LR
    AppKV["📱 App"] --> KV["🔑 Key-Value Store"]
    KV --> Get["GET usuario:123 → {nombre: 'Ana'}"]
    KV --> Set["SET usuario:123 → {datos}"]
    KV --> Del["DEL usuario:123"]

    style AppKV fill:#e1f5fe
    style KV fill:#fff3e0
```

### Redis

**Redis** es el rey de las key-value en memoria. Soporta estructuras de datos avanzadas (strings, lists, sets, sorted sets, hashes, streams).

```bash
# Strings
SET usuario:1:nombre "Ana"
GET usuario:1:nombre

# Hashes (objetos)
HSET usuario:1 id 1 nombre "Ana" email "ana@email.com"
HGETALL usuario:1

# Lists (colas)
LPUSH cola:tareas "procesar_pago"
RPOP cola:tareas

# Sets
SADD seguidores:1 "usuario:2" "usuario:3"
SMEMBERS seguidores:1

# Con expiración
SET cache:producto:42 "{...}" EX 3600
```

```mermaid
flowchart TB
    RedisUse["🔴 Usos de Redis"]
    RedisUse --> Cache["⚡ Caché de consultas"]
    RedisUse --> Sessions["🔐 Sesiones de usuario"]
    RedisUse --> RateLimit["⏱️ Rate Limiting"]
    RedisUse --> Queues["📋 Colas de tareas"]
    RedisUse --> PubSub["📡 Pub/Sub"]
    RedisUse --> Leaderboard["🏆 Leaderboards<br/>(Sorted Sets)"]

    style RedisUse fill:#dc382c,color:#fff
```

### DynamoDB

**DynamoDB** es la base de datos key-value/documento de AWS. Totalmente gestionada, escala automáticamente, con consistencia eventual o fuerte.

| Característica       | Redis                       | DynamoDB                    |
| -------------------- | --------------------------- | --------------------------- |
| **Tipo**             | En memoria                  | Disco SSD                   |
| **Persistencia**     | Opcional (RDB/AOF)          | Siempre                     |
| **Escalado**         | Manual (cluster)            | Automático                  |
| **Consistencia**     | Siempre consistente         | Eventual (o fuerte opcional)|
| **Modelo de datos**  | Estructuras complejas        | Documento / Key-Value       |
| **Caso típico**      | Caché, sesiones, colas      | Producción serverless       |

---

## Document DBs

Las bases de datos **documentales** almacenan datos en formato **JSON** o **BSON**. Cada documento puede tener una estructura diferente (schema flexible).

```json
// Documento MongoDB
{
    "_id": "abc123",
    "nombre": "Ana",
    "email": "ana@email.com",
    "direccion": {
        "calle": "Av. Siempre Viva",
        "ciudad": "Madrid"
    },
    "pedidos": [
        { "producto": "Laptop", "total": 1000 },
        { "producto": "Mouse", "total": 25 }
    ]
}
```

### MongoDB

**MongoDB** es la base de datos documental más popular. Ideal para aplicaciones con esquemas cambiantes, prototipado rápido y datos jerárquicos.

```javascript
// Insertar
db.usuarios.insertOne({
    nombre: "Ana",
    email: "ana@email.com",
    pedidos: []
});

// Buscar
db.usuarios.find({ email: "ana@email.com" });

// Buscar con filtro en array anidado
db.usuarios.find({
    "pedidos.producto": "Laptop"
});

// Agregación (similar a GROUP BY)
db.pedidos.aggregate([
    { $group: { _id: "$producto", total: { $sum: "$cantidad" } } }
]);
```

### CouchDB

**CouchDB** usa un modelo **MVCC** (Multi-Version Concurrency Control) y sincronización multi-maestro. Su superpoder: **sincronización offline** entre dispositivos.

- **AP** en CAP (Availability + Partition Tolerance)
- **Sincronización bidireccional** entre pares (PouchDB en el navegador ↔ CouchDB en servidor)
- Ideal para apps que funcionan **offline** y sincronizan cuando hay conexión

### MongoDB vs CouchDB

| Característica   | MongoDB                  | CouchDB                     |
| ---------------- | ------------------------ | --------------------------- |
| **CAP**          | CP (Consistencia + Partición) | AP (Disponibilidad + Partición) |
| **Sincronización** | Réplica maestro-esclavo | Multi-maestro (offline-first) |
| **Lenguaje consulta** | JSON queries, aggregation pipeline | MapReduce (views) |
| **Caso típico**  | APIs, catálogos, analítica | Apps offline-first, móviles |

---

## Column DBs (Anchos / Columnares)

Las bases de datos **columnares** almacenan datos por columna (no por fila). Son ideales para **analítica, agregaciones y grandes volúmenes**.

```mermaid
flowchart TB
    subgraph RowOriented ["📄 Row-Oriented (PostgreSQL)"]
        R1["Fila 1: id, nombre, email, fecha"]
        R2["Fila 2: id, nombre, email, fecha"]
        R3["Fila 3: id, nombre, email, fecha"]
        Note1["✅ Óptimo para muchas columnas<br/>de una misma fila (OLTP)"]
    end

    subgraph ColumnOriented ["📊 Column-Oriented (ClickHouse)"]
        C1["Columna id: 1, 2, 3, 4, 5..."]
        C2["Columna nombre: Ana, Bob, ..."]
        C3["Columna email: ana@, bob@..."]
        Note2["✅ Óptimo para agregaciones<br/>sobre una columna (OLAP)"]
    end

    style RowOriented fill:#e3f2fd
    style ColumnOriented fill:#c8e6c9
```

### ClickHouse

**ClickHouse** es un sistema de gestión de bases de datos **analítico** (OLAP) open source. Consultas SQL sobre miles de millones de filas en **milisegundos**.

```sql
-- ClickHouse: ideal para agregaciones masivas
SELECT
    toMonth(fecha) as mes,
    count() as visitas,
    sum(ingresos) as total_ingresos
FROM eventos
WHERE fecha >= now() - INTERVAL 1 YEAR
GROUP BY mes
ORDER BY mes;
```

### Cassandra

**Apache Cassandra** es una base de datos distribuida **peer-to-peer** diseñada para alta disponibilidad y escalabilidad lineal sin punto único de fallo.

```mermaid
flowchart TB
    subgraph CassandraArch ["🐘 Cassandra Ring"]
        direction LR
        N1["🖥️ Nodo 1"]
        N2["🖥️ Nodo 2"]
        N3["🖥️ Nodo 3"]
        N4["🖥️ Nodo 4"]
        N5["🖥️ Nodo 5"]
        N6["🖥️ Nodo 6"]

        N1 --- N2
        N2 --- N3
        N3 --- N4
        N4 --- N5
        N5 --- N6
        N6 --- N1
    end

    NoteCass["💡 Todos los nodos son iguales.<br/>No hay maestro. Datos replicados en N nodos."]

    style CassandraArch fill:#fce4ec
    style NoteCass fill:#f5f5f5
```

```cql
// Cassandra Query Language (CQL)
CREATE TABLE usuarios (
    id UUID PRIMARY KEY,
    nombre TEXT,
    email TEXT
);

SELECT * FROM usuarios WHERE id = ?;
```

### ScyllaDB

**ScyllaDB** es un reemplazo de Cassandra escrito en **C++** en vez de Java. Compatible con CQL pero **mucho más rápido** (10x).

| Característica | Cassandra             | ScyllaDB              |
| -------------- | --------------------- | --------------------- |
| **Lenguaje**   | Java                  | C++ (shard-per-core)  |
| **Rendimiento**| Bueno                 | 10x más rápido        |
| **Latencia**   | Mayor (GC de JVM)     | Mínima (sin GC)       |
| **Compatibilidad** | Original CQL      | Compatible CQL        |

---

## Graph DBs

Las bases de datos **de grafos** están diseñadas para datos **altamente relacionados**: redes sociales, motores de recomendación, detección de fraude.

```mermaid
flowchart TB
    subgraph GraphDB ["🕸️ Grafo de ejemplo"]
        Ana["👤 Ana"] -->|"SIGUE"| Bob["👤 Bob"]
        Ana -->|"COMPRÓ"| Laptop["💻 Laptop"]
        Bob -->|"COMPRÓ"| Mouse["🖱️ Mouse"]
        Laptop -->|"ES_CATEGORÍA"| Electronica["📱 Electrónica"]
        Mouse -->|"ES_CATEGORÍA"| Electronica
        Ana -->|"ESCRIBIÓ"| Review["📝 Review"]
        Review -->|"SOBRE"| Laptop
    end

    style GraphDB fill:#e8eaf6
```

### Neo4j

**Neo4j** es la base de datos de grafos más popular. Usa **Cypher** como lenguaje de consulta.

```cypher
// Crear nodos y relaciones
CREATE (ana:Usuario {nombre: 'Ana', edad: 25})
CREATE (bob:Usuario {nombre: 'Bob', edad: 30})
CREATE (ana)-[:SIGUE]->(bob)
CREATE (laptop:Producto {nombre: 'Laptop'})
CREATE (ana)-[:COMPRO {fecha: '2024-01-01'}]->(laptop)

// Consultar: ¿qué productos compraron los usuarios que sigo?
MATCH (yo:Usuario {nombre: 'Ana'})
      -[:SIGUE]->(amigo:Usuario)
      -[:COMPRO]->(producto:Producto)
RETURN amigo.nombre, producto.nombre
```

### AWS Neptune

**Neptune** es el servicio de base de datos de grafos gestionado de AWS. Soporta dos modelos: **Property Graph** (Gremlin/SPARQL) y **RDF** (SPARQL).

### DGraph

**DGraph** es una base de datos de grafos open source con **GraphQL nativo**. Escalabilidad horizontal y baja latencia.

```graphql
// DGraph: consultas en GraphQL
query {
    queryUsuario(filter: { nombre: { eq: "Ana" } }) {
        nombre
        sigue {
            nombre
            compro {
                nombre
            }
        }
    }
}
```

| Base de Datos | Lenguaje     | Escalabilidad | Mejor para                 |
| ------------- | ------------ | ------------- | -------------------------- |
| **Neo4j**     | Cypher       | Vertical      | Redes sociales, fraude     |
| **Neptune**   | Gremlin/SPARQL | Horizontal (AWS) | Apps AWS, RDF           |
| **DGraph**    | GraphQL      | Horizontal    | Apps modernas, GraphQL     |

---

## Time Series

Las bases de datos **de series temporales** están optimizadas para datos con **marca de tiempo**: métricas de servidores, sensores IoT, datos financieros.

### InfluxDB

**InfluxDB** es la time-series DB más popular. Usa su propio lenguaje de consulta (Flux) o SQL.

```sql
-- InfluxDB SQL
SELECT
    MEAN(temperature) as avg_temp,
    MAX(temperature) as max_temp
FROM sensor_data
WHERE time >= now() - INTERVAL '1 hour'
GROUP BY time(5m), device_id
```

### TimescaleDB

**TimescaleDB** es PostgreSQL con extensiones para time-series. Lo mejor de ambos mundos: **SQL completo + rendimiento time-series**.

```sql
-- TimescaleDB: SQL normal + hiper-tablas
CREATE TABLE sensor_data (
    time TIMESTAMPTZ NOT NULL,
    device_id INT,
    temperature FLOAT
);

-- Convertir a hiper-tabla particionada por tiempo
SELECT create_hypertable('sensor_data', 'time');

-- Consultas en tiempo real
SELECT time_bucket('5 minutes', time) as bucket,
       AVG(temperature)
FROM sensor_data
WHERE time > now() - INTERVAL '1 day'
GROUP BY bucket;
```

### InfluxDB vs TimescaleDB

| Característica   | InfluxDB                 | TimescaleDB               |
| ---------------- | ------------------------ | ------------------------- |
| **Motor**        | Propio (TSM)             | PostgreSQL + extensiones  |
| **SQL**          | Flux (similar a SQL)     | SQL completo              |
| **JOINs**        | ❌ Limitados              | ✅ Completo (PostgreSQL)  |
| **Ecosistema**   | Telegraf + Chronograf    | PostgreSQL + todo su ecosistema |
| **Retención**    | Nativa (automática)      | Manual (policies)         |
| **Compresión**   | Excelente                | Buena                     |

---

## Tiempo Real

Bases de datos diseñadas para **sincronizar datos en tiempo real** entre clientes y servidores.

### Firebase (Firestore)

**Firebase Firestore** es la base de datos en tiempo real de Google. Los clientes se **suscriben a documentos** y reciben cambios al instante.

```javascript
// Firebase: escuchar cambios en tiempo real
import { doc, onSnapshot } from "firebase/firestore";

const unsub = onSnapshot(doc(db, "chat", "sala-1"), (doc) => {
    console.log("Nuevos datos:", doc.data());
});

// Dejar de escuchar
unsub();
```

### RethinkDB

**RethinkDB** fue la primera DB en ofrecer **cambios en tiempo real** mediante un modelo push. Aunque su desarrollo comercial cesó, su influencia sigue viva en otras bases de datos.

```javascript
// RethinkDB: changefeeds
r.table('chat')
 .changes()
 .run(conn, (err, cursor) => {
     cursor.each((err, row) => {
         console.log('Cambio:', row.new_val);
     });
 });
```

---

## Comparativa general

```mermaid
flowchart TB
    Elegir2{"🔍 ¿Qué tipo de datos?"}

    Elegir2 -->|"Sesiones, caché,<br/>contadores"| KV2["🔑 Key-Value<br/>Redis, DynamoDB"]
    Elegir2 -->|"JSON flexible,<br/>esquema variable"| Doc2["📄 Documento<br/>MongoDB, CouchDB"]
    Elegir2 -->|"Analítica,<br/>grandes agregaciones"| Col2["📊 Columnar<br/>ClickHouse, Redshift"]
    Elegir2 -->|"Grafos, relaciones<br/>sociales, fraude"| Graph2["🕸️ Graph<br/>Neo4j, DGraph"]
    Elegir2 -->|"Métricas, sensores,<br/>monitoreo"| TS2["📈 Time Series<br/>InfluxDB, TimescaleDB"]
    Elegir2 -->|"Tiempo real,<br/>offline-first"| RT2["⚡ Tiempo Real<br/>Firebase, RethinkDB"]

    style Elegir2 fill:#e1f5fe
    style KV2 fill:#fff3e0
    style Doc2 fill:#c8e6c9
    style Col2 fill:#fce4ec
    style Graph2 fill:#e8eaf6
    style TS2 fill:#f3e5f5
    style RT2 fill:#fff9c4
```

### Tabla resumen

| Tipo         | Base de Datos   | Modelo          | CAP   | Caso de uso principal        |
| ------------ | --------------- | --------------- | ----- | ---------------------------- |
| **KV**       | Redis           | En memoria      | CP    | Caché, sesiones, colas       |
| **KV**       | DynamoDB        | Documento/KV    | AP    | Producción serverless        |
| **Documento**| MongoDB         | JSON            | CP    | APIs, prototipado            |
| **Documento**| CouchDB         | JSON            | AP    | Offline-first, móvil         |
| **Columnar** | Cassandra       | Wide column     | AP    | Altas escrituras, IoT        |
| **Columnar** | ClickHouse      | Columnar        | CP    | Analítica, OLAP              |
| **Graph**    | Neo4j           | Propiedad       | CP    | Redes sociales, fraude       |
| **Time Series** | InfluxDB    | TSM             | CP    | Métricas, monitoreo          |
| **Time Series** | TimescaleDB | SQL + hypertable | CA  | SQL + time-series            |
| **Tiempo Real** | Firebase    | Documento       | AP    | Apps en tiempo real          |

---

## Resumen visual

```mermaid
graph TB
    NoSQL3["🗄️ NoSQL"]

    NoSQL3 --> KV3["🔑 Key-Value<br/>Redis, DynamoDB<br/>⚡ Ultra rápido<br/>📋 Caché, sesiones"]

    NoSQL3 --> Doc3["📄 Documento<br/>MongoDB, CouchDB<br/>📦 JSON flexible<br/>🌐 APIs, catálogos"]

    NoSQL3 --> Col3["📊 Columnar<br/>Cassandra, ClickHouse<br/>📈 Altas escrituras<br/>📊 Analítica masiva"]

    NoSQL3 --> Graph3["🕸️ Graph<br/>Neo4j, DGraph<br/>🔗 Relaciones profundas<br/>👥 Redes sociales"]

    NoSQL3 --> TS3["📈 Time Series<br/>InfluxDB, TimescaleDB<br/>⏱️ Métricas temporales<br/>📡 IoT, monitoreo"]

    NoSQL3 --> RT3["⚡ Tiempo Real<br/>Firebase, RethinkDB<br/>🔄 Sincronización live<br/>📱 Apps offline-first"]

    style NoSQL3 fill:#e1f5fe
```

> **Siguiente paso:** Identifica un caso de uso en tu proyecto que no encaje bien en SQL. Por ejemplo: ¿necesitas caché? → Redis. ¿Datos JSON flexibles? → MongoDB. ¿Analítica rápida? → ClickHouse.

## Relacionados:
- [[escalando-bases-de-datos]] #anterior 
- [[habilidades-basicas-de-operaciones-devops]] #siguiente