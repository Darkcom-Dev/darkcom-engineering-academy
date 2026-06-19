# Escalando Bases de Datos

A medida que tu aplicación crece, la base de datos se convierte en el **cuello de botella** más común. Escalar una BD no es trivial: requiere estrategias de índices, replicación, particionamiento y entender las limitaciones teóricas (Teorema CAP).

```mermaid
flowchart TB
    Escalar["📈 Escalar BD"] --> Index["📈 Índices<br/>Optimizar consultas"]
    Escalar --> Replicacion["🔄 Replicación<br/>Copias de datos"]
    Escalar --> Sharding["🧩 Sharding<br/>Dividir datos"]
    Escalar --> CAP["🎯 Teorema CAP<br/>Consistencia vs Disponibilidad"]

    style Escalar fill:#e1f5fe
    style Index fill:#fff3e0
    style Replicacion fill:#c8e6c9
    style Sharding fill:#fce4ec
    style CAP fill:#e8eaf6
```

---

## Bases de datos indexadas

Un **índice** es una estructura de datos (B-tree, Hash) que acelera las búsquedas en una tabla a cambio de espacio en disco y velocidad de escritura.

```mermaid
flowchart LR
    subgraph SinIndice ["🚫 Sin índice (Seq Scan)"]
        Q1["SELECT * FROM usuarios<br/>WHERE email = 'a@b.com'"]
        Q1 --> Scan["Escanea TODAS las filas<br/>📊 1M filas → 1M lecturas"]
        Scan --> Slow["🐌 ~500ms"]
    end

    subgraph ConIndice ["✅ Con índice (Index Scan)"]
        Q2["SELECT * FROM usuarios<br/>WHERE email = 'a@b.com'"]
        Q2 --> IndexSearch["Busca en el índice B-tree<br/>📊 1M filas → ~20 lecturas"]
        IndexSearch --> Fast["⚡ ~1ms"]
    end

    style SinIndice fill:#ffcdd2
    style ConIndice fill:#c8e6c9
```

### Cuándo crear índices

```sql
-- ✅ Crear índice en columnas de filtro (WHERE)
CREATE INDEX idx_usuarios_email ON usuarios(email);

-- ✅ Índice compuesto para múltiples columnas
CREATE INDEX idx_pedidos_usuario_fecha
    ON pedidos(usuario_id, created_at);

-- ✅ Índice parcial (solo filas activas)
CREATE INDEX idx_pedidos_activos
    ON pedidos(usuario_id)
    WHERE estado = 'pendiente';

-- 📊 Ver índices existentes
-- PostgreSQL
SELECT * FROM pg_indexes WHERE tablename = 'usuarios';
```

### Cuándo NO crear índices

- Columnas con **pocos valores distintos** (booleanos, género)
- Tablas **pequeñas** (< 1000 filas)
- Columnas que se **actualizan frecuentemente**
- Tablas con **altas escrituras y bajas lecturas**

### Tipos de índices

| Tipo       | Uso principal                     | Velocidad           |
| ---------- | --------------------------------- | ------------------- |
| **B-tree** | Por defecto, búsquedas exactas y rango | ✅ Balanceado   |
| **Hash**   | Búsquedas de igualdad exacta      | ⚡ Muy rápido       |
| **GIN**    | Arrays, JSONB, full-text          | 🔍 Búsqueda textual |
| **GiST**   | Datos geométricos, búsqueda por rango | 🗺️ Geoespacial |

> **Regla:** Los índices aceleran lecturas (SELECT) pero ralentizan escrituras (INSERT/UPDATE/DELETE). No indexes todo.

---

## Replicación de datos

La **replicación** mantiene copias de los datos en múltiples servidores. Sirve para **alta disponibilidad**, **balanceo de lecturas** y **recuperación ante desastres**.

```mermaid
flowchart TB
    subgraph Replication ["🔄 Replicación"]
        Master["🖥️ Maestro<br/>(escribe)"]
        Replica1["🖥️ Réplica 1<br/>(solo lectura)"]
        Replica2["🖥️ Réplica 2<br/>(solo lectura)"]
        Replica3["🖥️ Réplica N<br/>(solo lectura)"]

        Master -->|"📤 WAL / Binlog"| Replica1
        Master -->|"📤"| Replica2
        Master -->|"📤"| Replica3
    end

    App["📱 App"] -->|"✏️ Escritura"| Master
    App -->|"📖 Lectura"| Replica1
    App -->|"📖 Lectura"| Replica2

    style Replication fill:#c8e6c9
    style Master fill:#fff3e0
    style App fill:#e1f5fe
```

### Topologías de replicación

```mermaid
flowchart TB
    subgraph Topologias ["📐 Topologías"]
        Single["🖥️ Single Leader<br/>Un maestro, N réplicas<br/>✅ Simple<br/>⚠️ Cuello de botella en escritura"]
        MultiLeader["🖥️🖥️ Multi-Leader<br/>Varios maestros<br/>✅ Escritura distribuida<br/>⚠️ Conflictos"]
        NoLeader["🖥️ Peer-to-Peer<br/>Todos escriben<br/>✅ Sin cuello de botella<br/>⚠️ Complejidad"]
    end

    style Topologias fill:#f5f5f5
    style Single fill:#e3f2fd
    style MultiLeader fill:#c8e6c9
    style NoLeader fill:#fff3e0
```

### Replicación síncrona vs asíncrona

| Tipo       | Confirmación                       | Riesgo                          |
| ---------- | ---------------------------------- | ------------------------------- |
| **Síncrona** | Escritura confirmada cuando todas las réplicas tienen el dato | Lento, pero cero pérdida |
| **Asíncrona** | Escritura confirmada al escribir en maestro | Rápido, pero posible pérdida si el maestro falla |

> **Caso típico:** 1 maestro para escrituras, N réplicas para lecturas. La app escribe en el maestro y lee de las réplicas (separa lecturas de escrituras).

---

## Estrategias de Sharding (Fragmentación)

El **sharding** (o fragmentación) **divide los datos horizontalmente** entre múltiples servidores. Cada servidor (shard) tiene un subconjunto de los datos.

```mermaid
flowchart TB
    subgraph Sharding ["🧩 Sharding"]
        App2["📱 App"]

        Router["🔀 Router / Proxy<br/>(determina qué shard)"]

        Shard1["💾 Shard 1<br/>usuarios id: 1-1000"]
        Shard2["💾 Shard 2<br/>usuarios id: 1001-2000"]
        Shard3["💾 Shard 3<br/>usuarios id: 2001-3000"]

        App2 --> Router
        Router --> Shard1
        Router --> Shard2
        Router --> Shard3
    end

    style Sharding fill:#fce4ec
    style Router fill:#e1f5fe
    style App2 fill:#fff3e0
```

### Estrategias de sharding

```mermaid
flowchart TB
    Estrategias["📐 Estrategias de Sharding"]

    Estrategias --> Range["📏 Por Rango<br/>usuarios id 1-1000 → shard 1<br/>usuarios id 1001-2000 → shard 2"]
    Estrategias --> Hash["#️⃣ Por Hash<br/>hash(usuario_id) % N → shard"]
    Estrategias --> Geo["🌍 Geográfica<br/>usuarios de MX → shard MX<br/>usuarios de ES → shard ES"]
    Estrategias --> Key["🔑 Basada en Clave<br/>tenant_id → shard<br/>(multi-tenant)"]

    Range --> R1["✅ Simple, range queries fáciles"]
    Range --> R2["⚠️ Datos pueden distribuirse desigualmente"]

    Hash --> H1["✅ Distribución uniforme"]
    Hash --> H2["⚠️ Añadir shards requiere re-hash o consistent hashing"]

    style Estrategias fill:#e1f5fe
```

### Desafíos del sharding

| Desafío              | Explicación                                    |
| -------------------- | ---------------------------------------------- |
| **Consultas cross-shard** | JOINs entre shards son lentos o imposibles   |
| **Re-balanceo**      | Añadir/quitar shards requiere redistribuir datos |
| **Transacciones**    | ACID entre shards es muy complejo              |
| **Auto-increment**   | IDs únicos globales requieren estrategia (UUID, Snowflake) |
| **Backup/Restore**   | Más complejo que una sola BD                   |

### Consistent Hashing

```mermaid
flowchart TB
    subgraph ConsistentHashing ["🎯 Consistent Hashing"]
        Ring["Anillo de hash"]
        N1["🖥️ Nodo 1"]
        N2["🖥️ Nodo 2"]
        N3["🖥️ Nodo 3"]
        N4["🖥️ Nodo 4"]

        K1["🔑 clave: usuario_123"] --> N1
        K2["🔑 clave: usuario_456"] --> N3
        K3["🔑 clave: usuario_789"] --> N2
    end

    Note["💡 Al añadir un nodo, solo se reubican<br/>las claves del rango afectado,<br/>NO todas las claves"]

    style ConsistentHashing fill:#e8eaf6
    style Note fill:#f5f5f5
```

> **Consistent Hashing** es la técnica que usan sistemas como Cassandra, DynamoDB y Redis Cluster para distribuir datos minimizando la relocalización al añadir/quitar nodos.

---

## Teorema CAP

El **Teorema CAP** (Brewer) dice que un sistema distribuido solo puede garantizar **2 de 3** propiedades simultáneamente:

```mermaid
graph TB
    CAP["🎯 Teorema CAP"]

    CAP --> C["✅ Consistency<br/>Todos ven los mismos<br/>datos al mismo tiempo"]
    CAP --> A["✅ Availability<br/>El sistema siempre<br/>responde"]
    CAP --> P["✅ Partition Tolerance<br/>El sistema funciona aunque<br/>haya fallos de red"]

    C --- CA["CP<br/>Consistencia +<br/>Tolerancia a partición<br/>Ej: HBase, MongoDB<br/>(sacrifican disponibilidad)"]
    A --- AP["AP<br/>Disponibilidad +<br/>Tolerancia a partición<br/>Ej: Cassandra, DynamoDB<br/>(sacrifican consistencia)"]
    P --- CP["CA<br/>Consistencia +<br/>Disponibilidad<br/>Ej: BD relacionales<br/>(sacrifican tolerancia)"]

    style CAP fill:#e8eaf6
    style C fill:#c8e6c9
    style A fill:#fff3e0
    style P fill:#fce4ec
```

### Explicación práctica

```mermaid
flowchart LR
    subgraph CAPExample ["💡 Ejemplo: Partición de red"]
        User1["👤 Usuario 1"] --> DB1["🗄️ BD EU"]
        User2["👤 Usuario 2"] --> DB2["🗄️ BD US"]

        DB1 -.-x|"💥 Cable roto"| DB2

        User1 -->|"✏️ Actualiza dato"| DB1
        User2 -->|"📖 Lee"| DB2

        DB2 -->|"CP: Rechazo la lectura<br/>(prefiero consistencia)"| CP2["⚠️ Error: no disponible"]
        DB2 -->|"AP: Devuelvo dato viejo<br/>(prefiero disponibilidad)"| AP2["✅ Dato puede ser viejo"]
    end

    style CAPExample fill:#f5f5f5
    style CP2 fill:#fff3e0
    style AP2 fill:#c8e6c9
```

### BD y su elección CAP

| Base de Datos       | Prioriza | Explicación                         |
| ------------------- | -------- | ----------------------------------- |
| **PostgreSQL**      | CA       | Consistencia + Disponibilidad (sacrifica tolerancia a partición) |
| **MongoDB**         | CP       | Consistencia + Tolerancia (si el primario cae, se elige nuevo) |
| **Cassandra**       | AP       | Disponibilidad + Tolerancia (consistencia eventual) |
| **DynamoDB**        | AP       | Disponibilidad + Tolerancia         |
| **Redis**           | CP       | Consistencia + Tolerancia (cluster) |

> **No significa que las BD CA no puedan particionarse.** Significa que en presencia de una partición de red, eligen negar la operación antes que devolver datos inconsistentes.

---

## Estrategia de escalado: orden de aplicación

```mermaid
flowchart TB
    P1["① Optimizar consultas<br/>+ índices"]
    P2["② Cachear (Redis)"]
    P3["③ Réplicas de lectura"]
    P4["④ Vertical scaling<br/>(más RAM/CPU al servidor)"]
    P5["⑤ Sharding horizontal"]

    P1 --> P2 --> P3 --> P4 --> P5

    Note["💡 Escala en este orden.<br/>La mayoría de apps nunca pasan del paso 3."]

    style P1 fill:#c8e6c9
    style P2 fill:#fff3e0
    style P3 fill:#e3f2fd
    style P4 fill:#fce4ec
    style P5 fill:#e8eaf6
    style Note fill:#f5f5f5
```

---

## Resumen visual

```mermaid
graph TB
    EscalarBD["📈 Escalar BD"]

    EscalarBD --> Indices2["📈 Índices<br/>B-tree, Hash, GIN<br/>Aceleran lecturas"]

    EscalarBD --> Replicacion2["🔄 Replicación<br/>Maestro → Réplicas<br/>Separa lectura/escritura"]

    EscalarBD --> Sharding2["🧩 Sharding<br/>Dividir datos en servidores<br/>Escalado horizontal real"]

    EscalarBD --> CAP2["🎯 Teorema CAP<br/>Consistencia vs Disponibilidad<br/>vs Tolerancia a partición"]

    EscalarBD --> Orden["📐 Orden de escalado<br/>1️⃣ Índices<br/>2️⃣ Caché<br/>3️⃣ Réplicas<br/>4️⃣ Sharding"]

    style EscalarBD fill:#e1f5fe
    style Indices2 fill:#fff3e0
    style Replicacion2 fill:#c8e6c9
    style Sharding2 fill:#fce4ec
    style CAP2 fill:#e8eaf6
    style Orden fill:#f5f5f5
```

> **Siguiente paso:** Identifica la consulta más lenta de tu app con EXPLAIN ANALYZE, créale un índice, y mide la mejora. Luego considera añadir una réplica de solo lectura para separar tráfico.

## Relacionado:
- [[data-en-tiempo-real]] #anterior 
- [[bases-de-datos-no-sql]] #siguiente