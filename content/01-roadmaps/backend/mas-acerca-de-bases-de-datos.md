# Más acerca de bases de datos

Más allá de las consultas básicas, las bases de datos relacionales tienen conceptos profundos que todo backend debe dominar para construir sistemas **confiables**, **rápidos** y **bien diseñados**.

```mermaid
graph TB
    BD["🗄️ Más sobre BD"] --> Transacciones["🔄 Transacciones<br/>Operaciones atómicas"]
    BD --> ORMs["🔗 ORMs<br/>Código vs SQL"]
    BD --> ACID["📋 ACID<br/>Garantías de integridad"]
    BD --> Normalizacion["📐 Normalización<br/>Diseño sin redundancia"]
    BD --> Failures["💥 Modos de fallo<br/>Qué sale mal"]
    BD --> Profiling["📊 Profiling<br/>Rendimiento y cuellos de botella"]

    style BD fill:#e1f5fe
    style Transacciones fill:#fff3e0
    style ORMs fill:#c8e6c9
    style ACID fill:#fce4ec
    style Normalizacion fill:#e8eaf6
    style Failures fill:#ffcdd2
    style Profiling fill:#e8f5e9
```

---

## Transacciones

Una **transacción** es un conjunto de operaciones que se ejecutan **como una sola unidad**: o se hacen **todas** o **ninguna**.

```mermaid
flowchart TB
    subgraph TransaccionExitosa ["✅ Transacción exitosa"]
        T1["BEGIN TRANSACTION"]
        T2["✏️ UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1"]
        T3["✏️ UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2"]
        T4["COMMIT"]
    end

    subgraph TransaccionFallida ["❌ Transacción fallida (rollback)"]
        T5["BEGIN TRANSACTION"]
        T6["✏️ UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1"]
        T7["💥 ¡Error! (servidor se cae,<br/>restricción violada)"]
        T8["↩️ ROLLBACK automático<br/>El saldo de la cuenta 1<br/>vuelve a su valor original"]
    end

    style TransaccionExitosa fill:#c8e6c9
    style TransaccionFallida fill:#ffcdd2
```

> **Analogía:** Transferencia bancaria. Si el sistema falla después de descontar de A pero antes de acreditar a B, el dinero no debe desaparecer. La transacción lo evita.

### Ejemplo SQL

```sql
BEGIN;

UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;

COMMIT;
-- Si algo falla antes del COMMIT,
-- todo se revierte automáticamente
```

```javascript
// Con ORM (Prisma)
await prisma.$transaction([
    prisma.cuenta.update({
        where: { id: 1 },
        data: { saldo: { decrement: 100 } }
    }),
    prisma.cuenta.update({
        where: { id: 2 },
        data: { saldo: { increment: 100 } }
    })
]);
```

### Niveles de aislamiento

Controlan cuánto "ve" una transacción de los cambios de otras transacciones concurrentes.

| Nivel               | Suciedad (dirty read) | No repetible | Fantasmas |
| ------------------- | --------------------- | ------------ | --------- |
| **READ UNCOMMITTED** | ❌ Puede leer datos no commitados | ❌ | ❌ |
| **READ COMMITTED**  | ✅ Previene           | ❌           | ❌        |
| **REPEATABLE READ** | ✅                    | ✅           | ❌        |
| **SERIALIZABLE**    | ✅                    | ✅           | ✅        |

---

## ACID

**ACID** es el acrónimo de las cuatro propiedades que garantizan que las transacciones sean confiables.

```mermaid
graph TB
    ACID2["📋 ACID"] --> A["Atomicity<br/>⚛️ Atomicidad<br/>Todo o nada"]
    ACID2 --> C["Consistency<br/>✅ Consistencia<br/>Los datos siempre<br/>cumplen las reglas"]
    ACID2 --> I["Isolation<br/>🔒 Aislamiento<br/>Transacciones concurrentes<br/>no se interfieren"]
    ACID2 --> D["Durability<br/>💾 Durabilidad<br/>Si se confirma,<br/>permanece aunque<br/>se caiga el sistema"]

    style ACID2 fill:#fce4ec
    style A fill:#ffcdd2
    style C fill:#c8e6c9
    style I fill:#e3f2fd
    style D fill:#fff3e0
```

| Propiedad    | Explicación                                      | Analogía                     |
| ------------ | ------------------------------------------------ | ---------------------------- |
| **Atomicidad** | La transacción se completa toda o se revierte toda | Un interruptor: o prende todo o no prende nada |
| **Consistencia** | Los datos siempre cumplen las reglas (claves, tipos, constraints) | Un libro de contabilidad que siempre cuadra |
| **Aislamiento** | Dos transacciones simultáneas no se ven entre sí hasta que terminan | Dos cajeros atendiendo a la vez sin mezclar clientes |
| **Durabilidad** | Una vez commiteado, el cambio sobrevive a un reinicio | Escribir con tinta indeleble, no con lápiz |

> **No todas las BD son iguales:** Las relacionales (PostgreSQL, MySQL) son ACID. Algunas NoSQL sacrifican ACID por rendimiento o escalabilidad (teorema CAP).

---

## ORMs (Object-Relational Mapping)

Un **ORM** es una capa que traduce entre tu **código orientado a objetos** y las **tablas relacionales**. Escribes en tu lenguaje y el ORM genera el SQL.

```mermaid
flowchart LR
    subgraph SinORM ["🚫 Sin ORM"]
        SQL["SELECT * FROM usuarios<br/>WHERE email = ?"]
        Code["📱 Código maneja<br/>strings SQL manualmente"]
    end

    subgraph ConORM ["✅ Con ORM"]
        Query["User.findByEmail('a@b.com')"]
        ORM["🔗 ORM traduce a SQL"]
        DB["🗄️ BD ejecuta"]
    end

    style SinORM fill:#ffcdd2
    style ConORM fill:#c8e6c9
```

### Ejemplos en distintos lenguajes

```javascript
// Prisma (Node.js / TypeScript)
const usuario = await prisma.user.findUnique({
    where: { email: 'ana@email.com' },
    include: { posts: true }
});
```

```python
# SQLAlchemy (Python)
usuario = session.query(User).filter(
    User.email == 'ana@email.com'
).first()
```

```ruby
# ActiveRecord (Ruby on Rails)
usuario = User.find_by(email: 'ana@email.com')
```

### Ventajas y desventajas

| Ventajas                         | Desventajas                         |
| -------------------------------- | ----------------------------------- |
| ✅ Productividad (menos SQL)      | ❌ Curva de aprendizaje             |
| ✅ Previene SQL injection         | ❌ Puede generar SQL ineficiente    |
| ✅ Migraciones versionadas        | ❌ Problema N+1 si no sabes usarlo  |
| ✅ Cambias de BD con mínimo esfuerzo | ❌ Menos control sobre queries complejas |

### ¿Cuándo usar ORM vs SQL puro?

```mermaid
flowchart TD
    Pregunta{"🔍 ¿La consulta es compleja?"}
    Pregunta -->|"CRUD simple"| ORM2["✅ Usa ORM<br/>find, create, update"]
    Pregunta -->|"Reporte, join <br/>complejo, agregación"| SQL2["✅ SQL directo<br/>RAW query / view"]

    style Pregunta fill:#e1f5fe
    style ORM2 fill:#c8e6c9
    style SQL2 fill:#fff3e0
```

---

## Normalización

La **normalización** es el proceso de diseñar tablas para **eliminar redundancia** y **evitar anomalías** al insertar, actualizar o eliminar datos.

```mermaid
graph TB
    subgraph Desnormalizado ["🚫 Tabla única (redundancia)"]
        T1["| orden_id | cliente | producto | precio | categoria |"]
        T1_1["| 1 | Ana | Laptop | 1000 | Electrónica |"]
        T1_2["| 2 | Ana | Mouse  | 20   | Electrónica |"]
        T1_3["| 3 | Bob  | Laptop | 1000 | Electrónica |"]
        Note1["⚠️ 'Ana' repetido<br/>⚠️ 'Laptop' y precio repetido<br/>⚠️ Si cambia el precio,<br/>hay que cambiar en N filas"]
    end

    subgraph Normalizado ["✅ Tablas normalizadas"]
        T2_clientes["👤 clientes<br/>| id | nombre |"]
        T2_productos["📦 productos<br/>| id | nombre | precio | categoria_id |"]
        T2_categorias["📂 categorias<br/>| id | nombre |"]
        T2_ordenes["🧾 ordenes<br/>| id | cliente_id | fecha |"]
        T2_detalles["🔗 detalle_orden<br/>| orden_id | producto_id | cantidad |"]
    end

    style Desnormalizado fill:#ffcdd2
    style Normalizado fill:#c8e6c9
```

### Formas normales

| Forma     | Regla                                       |
| --------- | ------------------------------------------- |
| **1NF**   | Cada celda = un valor atómico. No listas    |
| **2NF**   | 1NF + cada columna no clave depende de TODA la clave primaria |
| **3NF**   | 2NF + no hay dependencias transitivas (A→B→C) |

**En la práctica:** llegar a 3NF es suficiente para la mayoría de los casos. A veces se **desnormaliza** intencionalmente por rendimiento (evitar joins costosos).

```mermaid
flowchart LR
    subgraph Normalizar ["📐 Niveles de normalización"]
        NF1["1NF: Valores atómicos"] --> NF2["2NF: Sin dependencias parciales"]
        NF2 --> NF3["3NF: Sin dependencias transitivas"]
        NF3 --> NFBC["BCNF: Toda dependencia<br/>es de clave"]
    end

    Note["💡 3NF suele ser suficiente<br/>para la mayoría de apps"]
    NF3 --> Note

    style Normalizar fill:#e8eaf6
    style Note fill:#f5f5f5
```

---

## Failure Modes (Modos de fallo)

Las bases de datos pueden fallar de muchas formas. Conocer estos modos te ayuda a diseñar sistemas resilientes.

```mermaid
flowchart TB
    subgraph FailureModes ["💥 Modos de fallo comunes"]
        Connection["🔌 Pérdida de conexión<br/>El servidor de BD no responde"]
        Timeout["⏰ Timeout<br/>La consulta tarda demasiado"]
        Deadlock["🔒 Deadlock<br/>Dos transacciones esperan<br/>la una por la otra"]
        Constraint["🚫 Violación de constraint<br/>FK, unique, not null"]
        Disk["💾 Disco lleno<br/>La BD no puede escribir"]
        Corrupt["🧨 Corrupción de datos<br/>El archivo de BD se daña"]
        Replication["📡 Replication lag<br/>Réplica no está al día"]
    end

    style FailureModes fill:#ffcdd2
```

### Estrategias de mitigación

```mermaid
flowchart LR
    Fallo["💥 Fallo"] --> Estrategia["🛡️ Estrategia"]
    Estrategia --> Retry["🔄 Reintento con backoff<br/>Para timeouts y conexión"]
    Estrategia --> Circuit["🔌 Circuit breaker<br/>Dejar de intentar tras N fallos"]
    Estrategia --> Pool["🧰 Connection pool<br/>Gestionar conexiones"]
    Estrategia --> Backup["💾 Backups automáticos<br/>Para corrupción o error humano"]
    Estrategia --> Monitor["📊 Monitoreo<br/>Alertas de disco, conexiones, réplicas"]

    style Fallo fill:#ffcdd2
    style Estrategia fill:#c8e6c9
```

### Código resiliente

```javascript
// Manejo de fallos con retry
async function queryWithRetry(sql, params, retries = 3) {
    for (let i = 0; i < retries; i++) {
        try {
            return await db.query(sql, params);
        } catch (err) {
            if (i === retries - 1) throw err;
            if (err.code === 'CONNECTION_ERROR' || err.code === 'DEADLOCK') {
                console.log(`Reintento ${i + 1}/${retries}`);
                await sleep(1000 * Math.pow(2, i)); // backoff exponencial
            } else {
                throw err; // errores no recuperables
            }
        }
    }
}
```

---

## Perfilando el desempeño (Profiling Performance)

El **profiling** consiste en identificar consultas lentas, cuellos de botella y oportunidades de optimización.

```mermaid
flowchart TB
    Slow["🐌 App lenta"] --> Profile["🔍 Perfilar"]
    Profile --> Identify["🔎 Identificar la consulta lenta"]
    Identify --> Analyze["📊 Analizar con EXPLAIN"]
    Analyze --> Fix["🔧 Optimizar"]
    Fix --> Verify["✅ Verificar mejora"]

    subgraph Tools ["🛠️ Herramientas"]
        Log["📋 Slow query log<br/>(PostgreSQL: log_min_duration)"]
        Explain["📊 EXPLAIN ANALYZE<br/>Ver plan de ejecución"]
        Index["📈 pg_stat_user_indexes<br/>Índices usados vs no usados"]
        Monitor2["📊 pg_stat_activity<br/>Consultas en ejecución"]
    end

    Profile --> Tools

    style Slow fill:#ffcdd2
    style Profile fill:#fff3e0
    style Identify fill:#e8eaf6
    style Analyze fill:#c8e6c9
    style Fix fill:#e3f2fd
    style Verify fill:#c8e6c9
    style Tools fill:#f5f5f5
```

### Cómo leer un plan de ejecución (EXPLAIN)

```sql
EXPLAIN ANALYZE
SELECT u.nombre, COUNT(p.id) as total_posts
FROM usuarios u
LEFT JOIN posts p ON p.usuario_id = u.id
WHERE u.activo = true
GROUP BY u.id;
```

```
                                        QUERY PLAN
-------------------------------------------------------------------
 GroupAggregate  (cost=125.00..250.00 rows=1000 width=40)
   ->  Hash Left Join  (cost=100.00..200.00 rows=5000 width=36)
         Hash Cond: (u.id = p.usuario_id)
         ->  Seq Scan on usuarios u  (cost=0.00..50.00 rows=1000 width=32)
               Filter: activo = true
         ->  Hash  (cost=50.00..50.00 rows=5000 width=8)
               ->  Seq Scan on posts p  (cost=0.00..50.00 rows=5000 width=8)
 Planning Time: 0.500 ms
 Execution Time: 15.200 ms
```

```mermaid
flowchart LR
    subgraph LecturaExplain ["📖 Cómo leer EXPLAIN"]
        Cost["cost=125.00..250.00<br/>⏱️ Costo estimado<br/>(menor = mejor)"]
        Rows["rows=1000<br/>📊 Filas estimadas<br/>(vs reales)"]
        Type["Hash Left Join<br/>🔗 Tipo de join"]
        Time["Execution Time: 15ms<br/>⏱️ Tiempo real"]
    end

    style LecturaExplain fill:#e8eaf6
```

### Estrategias de optimización

```mermaid
graph TB
    subgraph Optimizaciones ["🔧 Cómo optimizar"]
        Index["📈 Añadir índices<br/>En columnas usadas en WHERE,<br/>JOIN, ORDER BY"]
        Denorm["📐 Desnormalización<br/>Evitar joins costosos<br/>(con cuidado)"]
        Partition["📦 Particionamiento<br/>Dividir tablas grandes<br/>por fecha o región"]
        Cache["⚡ Cachear resultados<br/>Redis para consultas<br/>frecuentes y lentas"]
        Pagination["📄 Paginación<br/>LIMIT + OFFSET vs<br/>cursor-based"]
        Materialized["📋 Vistas materializadas<br/>Resultados precalculados"]
    end

    style Optimizaciones fill:#c8e6c9
```

### Reglas de oro de performance

1. **Mide antes de optimizar** — no adivines, usa EXPLAIN ANALYZE
2. **Índices en WHERE y JOIN** — las columnas de filtrado y unión
3. **Cuidado con SELECT \*** — solo pide las columnas que necesitas
4. **N+1 es el enemigo #1** — usa JOIN o eager loading
5. **Paginación cursor-based** para tablas grandes (evita OFFSET)
6. **Cachea lo que se repite** — si una consulta se ejecuta 1000 veces/min, cachea

---

## Resumen visual

```mermaid
graph TB
    MasBD["🗄️ Más sobre BD"] --> Trans["🔄 Transacciones<br/>BEGIN / COMMIT / ROLLBACK"]
    MasBD --> ACID3["📋 ACID<br/>Atomicidad, Consistencia,<br/>Aislamiento, Durabilidad"]
    MasBD --> ORM3["🔗 ORMs<br/>Objetos → Tablas"]
    MasBD --> Norm["📐 Normalización<br/>1NF → 2NF → 3NF"]
    MasBD --> Fail["💥 Failure Modes<br/>Conexión, deadlock,<br/>corrupción, timeout"]
    MasBD --> Perf["📊 Profiling<br/>EXPLAIN, índices,<br/>caché, paginación"]

    style MasBD fill:#e1f5fe
    style Trans fill:#fff3e0
    style ACID3 fill:#fce4ec
    style ORM3 fill:#c8e6c9
    style Norm fill:#e8eaf6
    style Fail fill:#ffcdd2
    style Perf fill:#e8f5e9
```

> **Siguiente paso:** Implementa transacciones en tu próxima feature que modifique múltiples tablas. Agrega índices a las columnas que más consultas. Y habilita el slow query log para encontrar cuellos de botella ocultos.

## Relacionados:
- [[ci-cd]] #anterior 
- [[testing]] #siguiente 