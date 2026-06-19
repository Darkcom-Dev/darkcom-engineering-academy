# Caching

El **caching** es la técnica de almacenar datos temporalmente en un lugar de acceso rápido para evitar tener que generarlos o buscarlos repetidamente. Es una de las estrategias más efectivas para mejorar rendimiento.

```mermaid
flowchart LR
    Cliente["👤 Usuario"] -->|"Solicitud"| Cache["⚡ Caché"]
    Cache -->|"¿Tengo el dato?"| Decision{"Hit o Miss?"}

    Decision -->|"✅ HIT → dato en caché"| Cache
    Decision -->|"❌ MISS → dato no está"| Server["🖥️ Servidor / BD"]

    Server --> Cache
    Cache --> Cliente

    style Cliente fill:#e1f5fe
    style Cache fill:#fff3e0
    style Decision fill:#fff9c4
    style Server fill:#c8e6c9
```

> **Analogía:** Caching es como tener los ingredientes que más usas en la encimera de la cocina, en vez de ir al refrigerador cada vez que los necesitas.

### Conceptos clave

| Término   | Significado                                    |
| --------- | ---------------------------------------------- |
| **Cache Hit**  | El dato está en caché → respuesta rápida   |
| **Cache Miss** | El dato no está → hay que ir al origen     |
| **TTL**   | Time To Live — tiempo que vive el dato en caché |
| **Expiración** | El dato se elimina automáticamente al vencer |
| **Invalidación** | Se elimina el dato porque cambió el original |
| **Evicción** | Se elimina un dato para hacer espacio al nuevo |

### Estrategias de caché

```mermaid
flowchart TB
    subgraph Estrategias ["📐 Estrategias de Caché"]
        CacheAside["🔀 Cache-Aside (Lazy Loading)<br/>La app pregunta primero a caché,<br/>si falla va a la BD y guarda"]
        ReadThrough["📖 Read-Through<br/>El caché pregunta a la BD<br/>automáticamente si no tiene el dato"]
        WriteAround["✏️ Write-Around<br/>Escribe directo en BD,<br/>invalida caché"]
        WriteThrough["📝 Write-Through<br/>Escribe en BD y caché<br/>a la vez"]
        WriteBack["⏳ Write-Back<br/>Escribe en caché primero,<br/>luego sincroniza con BD"]
    end

    style Estrategias fill:#f5f5f5
    style CacheAside fill:#e3f2fd
    style ReadThrough fill:#c8e6c9
    style WriteAround fill:#fff3e0
    style WriteThrough fill:#fce4ec
    style WriteBack fill:#e8eaf6
```

### ¿Qué cachear?

```mermaid
flowchart TD
    Pregunta{"🔍 ¿Este dato cambia<br/>con frecuencia?"}

    Pregunta -->|"Sí, cambia cada segundo"| No["❌ NO cachear<br/>Ej: precio de acción en bolsa"]
    Pregunta -->|"Cambia poco"| Si["✅ SÍ cachear"]

    Si --> Q2{"¿Es costoso de<br/>generar o consultar?"}
    Q2 -->|"Sí"| Si2["✅ Muy buen candidato"]
    Q2 -->|"No"| Q3{"¿Se consulta<br/>mucho?"}
    Q3 -->|"Sí"| Si2
    Q3 -->|"No"| No2["❌ No vale la pena"]

    style Pregunta fill:#e1f5fe
    style Si fill:#c8e6c9
    style Si2 fill:#c8e6c9
    style No fill:#ffcdd2
    style No2 fill:#ffcdd2
```

---

## HTTP Caching

El **caching HTTP** ocurve en el navegador o en proxies intermedios (CDN). Las respuestas HTTP incluyen **cabeceras** que controlan cómo y por cuánto tiempo se pueden cachear.

```mermaid
sequenceDiagram
    participant Browser as 🌍 Navegador
    participant Cache as 💾 Caché del navegador
    participant Server as 🖥️ Servidor

    Browser->>Server: ① GET /imagen.jpg
    Server-->>Browser: ② 200 OK + imagen<br/>Cache-Control: max-age=3600
    Browser->>Cache: ③ Guarda imagen por 1 hora

    Note over Browser,Browser: Pasan 30 minutos...

    Browser->>Cache: ④ GET /imagen.jpg
    Cache-->>Browser: ⑤ ✅ HIT (desde caché local)
    Note right of Browser: Respuesta instantánea (0ms)

    Note over Browser,Browser: Pasa 1 hora...

    Browser->>Server: ⑥ GET /imagen.jpg<br/>If-None-Match: "abc123"
    Server-->>Browser: ⑦ 304 Not Modified<br/>(sin cuerpo)
    Browser->>Cache: ⑧ Renueva TTL
```

### Cabeceras principales

```mermaid
flowchart LR
    HTTPC["📋 Cabeceras de Caché HTTP"]

    HTTPC --> CacheControl["Cache-Control<br/>Controla el comportamiento"]

    CacheControl --> Public["public<br/>Puede cachear cualquiera<br/>(navegador + CDN)"]
    CacheControl --> Private["private<br/>Solo el navegador<br/>(no CDN/intermediarios)"]
    CacheControl --> MaxAge["max-age=3600<br/>Tiempo de vida en segundos"]
    CacheControl --> NoCache["no-cache<br/>Revalidar con servidor<br/>antes de usar"]
    CacheControl --> NoStore["no-store<br/>No cachear NADA"]

    HTTPC --> ETag["ETag<br/>Hash del contenido<br/>→ If-None-Match"]
    HTTPC --> LastModified["Last-Modified<br/>Fecha del recurso<br/>→ If-Modified-Since"]

    style HTTPC fill:#e1f5fe
    style CacheControl fill:#fff3e0
    style ETag fill:#c8e6c9
    style LastModified fill:#fce4ec
```

### Ejemplo de cabeceras

```http
# Respuesta cacheable por 1 hora
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: public, max-age=3600
ETag: "abc123"
Last-Modified: Tue, 15 May 2025 12:00:00 GMT

# El navegador revalida con:
GET /api/usuarios
If-None-Match: "abc123"
If-Modified-Since: Tue, 15 May 2025 12:00:00 GMT

# Respuesta si no cambió:
HTTP/1.1 304 Not Modified
# (sin cuerpo — ahorra ancho de banda)
```

---

## Caché del lado del servidor

La caché del lado del servidor almacena datos en **memoria RAM** (mucho más rápida que el disco) para evitar consultas repetitivas a la base de datos o cálculos costosos.

```mermaid
flowchart LR
    App["📱 App"] --> Cache["⚡ Caché en RAM"]
    Cache -->|"Hit"| App
    Cache -->|"Miss"| BD["🗄️ Base de Datos"]
    BD --> Cache

    Note right of Cache: ~0.1ms por lectura
    Note right of BD: ~10-50ms por consulta

    style App fill:#e1f5fe
    style Cache fill:#fff3e0
    style BD fill:#c8e6c9
```

### Casos de uso típicos

- **Páginas enteras:** HTML renderizado que cambia poco (home, landing pages)
- **Fragmentos:** Sidebars, menús, configuraciones
- **Resultados de consultas:** Listados paginados que se repiten
- **Sesiones:** Datos de sesión de usuario (evitar consultar BD en cada request)
- **Rate limiting:** Contadores para limitar peticiones por IP

---

## Redis

**Redis** es un almacén de datos en memoria, de tipo **clave-valor**, que se usa principalmente como caché, cola de mensajes y base de datos ligera. Es el rey del caching moderno.

```mermaid
flowchart TB
    subgraph RedisFeatures ["⚡ Redis"]
        Strings["🔤 Strings<br/>SET / GET"]
        Lists["📋 Lists<br/>LPUSH / RPOP"]
        Sets["🎯 Sets<br/>SADD / SMEMBERS"]
        Hashes["🗂️ Hashes<br/>HSET / HGET"]
        SortedSets["🏆 Sorted Sets<br/>ZADD / ZRANGE"]
        PubSub["📡 Pub/Sub<br/>PUBLISH / SUBSCRIBE"]
        Streams["🌊 Streams<br/>XADD / XREAD"]
    end

    style RedisFeatures fill:#dc382c,color:#fff
```

### Comandos básicos

```bash
# Strings (el tipo más simple)
SET usuario:1:nombre "Ana"
GET usuario:1:nombre          # → "Ana"
TTL usuario:1:nombre           # → -1 (no expira)
EXPIRE usuario:1:nombre 3600   # → expira en 1 hora

# Con expiración en el SET
SET usuario:1:session "token123" EX 3600

# Hashes (objetos anidados)
HSET usuario:1 id 1 nombre "Ana" email "ana@email.com"
HGET usuario:1 nombre           # → "Ana"
HGETALL usuario:1               # → todos los campos

# Lists (colas)
LPUSH cola:tareas "procesar_pago"
RPOP cola:tareas                # → "procesar_pago"

# Pub/Sub (mensajería)
PUBLISH canal:noticias "nuevo artículo"
SUBSCRIBE canal:noticias
```

### Patrón típico con Redis

```javascript
// Pseudocódigo: Cache-Aside con Redis
function getUsuario(id) {
    const cacheKey = `usuario:${id}`;

    // 1. Intentar obtener de caché
    const cached = redis.get(cacheKey);
    if (cached) {
        return JSON.parse(cached);          // ✅ HIT
    }

    // 2. Si no está, ir a la BD
    const usuario = db.query(
        'SELECT * FROM usuarios WHERE id = ?', [id]
    );

    // 3. Guardar en caché por 5 minutos
    redis.set(cacheKey, JSON.stringify(usuario), 'EX', 300);

    return usuario;                          // ⚠️ MISS
}
```

```mermaid
sequenceDiagram
    participant App as 📱 App
    participant Redis as ⚡ Redis
    participant DB as 🗄️ Base de Datos

    App->>Redis: GET usuario:1
    alt Cache Hit
        Redis-->>App: ✅ Datos del usuario
    else Cache Miss
        Redis-->>App: ❌ nil (no existe)
        App->>DB: SELECT * FROM usuarios WHERE id=1
        DB-->>App: Datos del usuario
        App->>Redis: SET usuario:1 [datos] EX 300
    end
```

### Características clave

| Característica      | Redis                        |
| ------------------- | ---------------------------- |
| **Persistencia**    | Opcional (RDB / AOF)         |
| **Estructuras**     | Strings, Lists, Sets, Hashes, Sorted Sets, Streams, HyperLogLog, Bitmaps |
| **Replicación**     | Maestro-esclavo, Sentinel, Cluster |
| **Velocidad**       | ~100,000 ops/seg (en memoria) |
| **Usos**            | Caché, sesiones, colas, rate limiter, pub/sub |

---

## Memcached

**Memcached** es un sistema de caché distribuida en memoria, también clave-valor pero **más simple** que Redis. Fue el estándar antes de que Redis ganara popularidad.

```mermaid
flowchart LR
    subgraph MemcachedFeatures ["🔵 Memcached"]
        Simple["✅ Simple<br/>Solo strings"]
        Multi["✅ Multi-get<br/>Obtener varias claves"]
        TTL["✅ Expiración automática<br/>LRU eviction"]
        MultiNode["✅ Distribuido<br/>Varios servidores"]
    end

    subgraph NoTiene ["❌ No tiene"]
        Persist["❌ Persistencia<br/>(solo en RAM)"]
        Complex["❌ Estructuras complejas<br/>(solo clave-valor)"]
        PubSub["❌ Pub/Sub"]
        Replication["❌ Replicación"]
    end

    style MemcachedFeatures fill:#e3f2fd
    style NoTiene fill:#ffcdd2
```

### Comandos básicos

```bash
# Guardar y obtener
set usuario:1 0 3600 15
Ana|ana@email.com
STORED

get usuario:1  # → "Ana|ana@email.com"

# Eliminar
delete usuario:1

# Incrementar (útil para contadores)
incr contador:visitas 1
```

### Redis vs Memcached

```mermaid
flowchart TB
    subgraph Comparativa ["⚖️ Redis vs Memcached"]
        Redis2["🟥 Redis"] --> R1["✅ Estructuras complejas"]
        Redis2 --> R2["✅ Persistencia opcional"]
        Redis2 --> R3["✅ Pub/Sub, Streams"]
        Redis2 --> R4["✅ Replicación y Cluster"]
        Redis2 --> R5["⚠️ Más complejo de operar"]

        Mem2["🔵 Memcached"] --> M1["✅ Multi-thread (usa más CPUs)"]
        Mem2 --> M2["✅ Más simple, menos overhead"]
        Mem2 --> M3["⚠️ Solo strings"]
        Mem2 --> M4["⚠️ Sin persistencia"]
        Mem2 --> M5["⚠️ Sin replicación nativa"]
    end

    style Comparativa fill:#f5f5f5
    style Redis2 fill:#dc382c,color:#fff
    style Mem2 fill:#1565c0,color:#fff
```

**¿Cuándo usar Memcached hoy?** En casos muy simples donde solo necesitas un caché clave-valor, multi-thread y no necesitas persistencia ni estructuras complejas. Redis es casi siempre la mejor opción hoy en día.

---

## Niveles de caché

```mermaid
flowchart TB
    N1["🏠 Nivel 1: Navegador<br/>Cache-Control, ETag<br/>~0ms"]
    N2["🌍 Nivel 2: CDN<br/>CloudFlare, Akamai, Fastly<br/>~5ms"]
    N3["⚡ Nivel 3: Redis / Memcached<br/>Caché de aplicación<br/>~0.1ms"]
    N4["🗄️ Nivel 4: Base de Datos<br/>Query cache, índices<br/>~10ms"]
    N5["💾 Nivel 5: Disco<br/>Archivos, SSD, almacenamiento<br/>~50ms+"]

    Cliente["👤 Usuario"] --> N1
    N1 --> N2
    N2 --> N3
    N3 --> N4
    N4 --> N5

    style Cliente fill:#e1f5fe
    style N1 fill:#fff3e0
    style N2 fill:#fce4ec
    style N3 fill:#dc382c,color:#fff
    style N4 fill:#c8e6c9
    style N5 fill:#f5f5f5
```

---

## Buenas prácticas

- **No cachees todo:** Evalúa frecuencia de cambio y costo de generar el dato
- **TTL razonable:** Ni muy corto (pierdes beneficio) ni muy largo (datos obsoletos)
- **Invalidación manual:** Cuando un dato cambia, elimínalo de caché (o actualízalo)
- **Key naming consistente:** `entidad:id:campo` (ej: `usuario:1:nombre`)
- **Cacheo de páginas vs fragmentos:** Prefiere fragmentos pequeños y reutilizables
- **Monitoreo:** Mide hit ratio, latencia y memoria usada
- **Cache stampede:** Cuando expira un dato popular y N peticiones intentan regenerarlo a la vez → usa bloqueo (lock) o TTL escalonado

---

## Resumen visual

```mermaid
flowchart TB
    Cache["⚡ Caching"] --> HTTP["🌐 HTTP Caching"]
    Cache --> ServerSide["🖥️ Server-Side Cache"]

    HTTP --> Browser["🌍 Navegador<br/>Cache-Control, ETag"]
    HTTP --> CDN["🌍 CDN<br/>CloudFlare, Fastly"]

    ServerSide --> Redis["🟥 Redis<br/>Clave-valor avanzado<br/>Estructuras, Pub/Sub, Persistencia"]
    ServerSide --> Memcached["🔵 Memcached<br/>Clave-valor simple<br/>Multi-thread, sin persistencia"]

    Redis --> Use1["✅ Caché de consultas"]
    Redis --> Use2["✅ Sesiones de usuario"]
    Redis --> Use3["✅ Colas de tareas"]
    Redis --> Use4["✅ Rate limiting"]
    Redis --> Use5["✅ Pub/Sub en tiempo real"]

    style Cache fill:#e1f5fe
    style HTTP fill:#e8f5e9
    style ServerSide fill:#fff3e0
    style Redis fill:#dc382c,color:#fff
    style Memcached fill:#1565c0,color:#fff
```

---

> **Siguiente paso:** Instala Redis, juega con `redis-cli`, e intégralo en tu backend con la librería `ioredis` (Node.js) o `redis-py` (Python).

## Relacionados:
- [[aprende-acerca-de-apis]] #anterior 
- [[servidores-web]] #siguiente 