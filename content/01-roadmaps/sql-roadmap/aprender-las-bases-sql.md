# Aprender las bases

## ¿Qué son bases de datos relacionales?

Una base de datos relacional es un tipo de base de datos que almacena y organiza los datos en **tablas** (relaciones) que se conectan entre sí mediante **claves** comunes.

```mermaid
flowchart LR
    A[Base de Datos Relacional] --> B[Tablas]
    B --> C[Filas / Registros]
    B --> D[Columnas / Campos]
    A --> E[Relaciones entre tablas]
    E --> F[Clave Primaria PK]
    E --> G[Clave Foránea FK]
```

### Conceptos clave

| Concepto | Definición | Ejemplo |
|---|---|---|
| **Tabla** | Colección de datos organizados en filas y columnas | `usuarios`, `pedidos` |
| **Fila / Registro** | Un elemento completo de datos | Un usuario con nombre, email, edad |
| **Columna / Campo** | Un atributo específico del dato | `nombre`, `email` |
| **Clave Primaria (PK)** | Identificador único de cada fila | `id INT PRIMARY KEY` |
| **Clave Foránea (FK)** | Campo que referencia la PK de otra tabla | `usuario_id` en `pedidos` → `id` en `usuarios` |
| **Índice** | Estructura que acelera las búsquedas | Índice sobre `email` para buscar rápido |

### Ejemplo visual de tablas relacionadas

```mermaid
erDiagram
    CLIENTES {
        int id PK
        string nombre
        string email
    }
    PEDIDOS {
        int id PK
        date fecha
        float total
        int cliente_id FK
    }
    CLIENTES ||--o{ PEDIDOS : "realiza"
```

**Lectura del diagrama:** Un cliente puede realizar **muchos** pedidos. Cada pedido pertenece a **un solo** cliente.

### Lenguaje SQL

Para trabajar con bases de datos relacionales usamos **SQL** (Structured Query Language). Sus operaciones principales son:

```mermaid
flowchart LR
    SQL[SQL] --> DDL[DDL - Definición]
    SQL --> DML[DML - Manipulación]
    SQL --> DQL[DQL - Consulta]
    SQL --> DCL[DCL - Control]

    DDL --> CREATE[CREATE TABLE]
    DDL --> ALTER[ALTER TABLE]
    DDL --> DROP[DROP TABLE]

    DML --> INSERT[INSERT INTO]
    DML --> UPDATE[UPDATE]
    DML --> DELETE[DELETE]

    DQL --> SELECT[SELECT]

    DCL --> GRANT[GRANT]
    DCL --> REVOKE[REVOKE]
```

**Ejemplo básico:**
```sql
-- Crear tabla
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    email VARCHAR(100) UNIQUE
);

-- Insertar datos
INSERT INTO clientes (nombre, email) VALUES ('Ana', 'ana@email.com');

-- Consultar
SELECT * FROM clientes WHERE email = 'ana@email.com';
```

---

## Beneficios y limitaciones de RDBMS

```mermaid
flowchart TD
    RDBMS[RDBMS] --> Pro["✅ Beneficios"]
    RDBMS --> Contra["❌ Limitaciones"]

    Pro --> P1[Integridad de datos]
    Pro --> P2[Consultas complejas con JOIN]
    Pro --> P3[ACID - Transacciones seguras]
    Pro --> P4[Estandarización SQL]
    Pro --> P5[Sin redundancia - Normalización]

    Contra --> C1[Dificultad para escalar horizontalmente]
    Contra --> C2[Modelo rígido - esquema fijo]
    Contra --> C3[Rendimiento con datos no estructurados]
    Contra --> C4[Operaciones costosas en clusters grandes]
```

### Beneficios detallados

| Beneficio | Explicación |
|---|---|
| **Integridad de datos** | Las restricciones (PK, FK, UNIQUE, CHECK) evitan datos inválidos |
| **Transacciones ACID** | Atomicidad, Consistencia, Aislamiento, Durabilidad — las operaciones se ejecutan de forma segura |
| **Sin redundancia** | La normalización elimina datos duplicados |
| **Consultas complejas** | Con JOINs puedes relacionar múltiples tablas en una sola consulta |
| **Madurez** | Tecnología con décadas de desarrollo, muy estable y documentada |

### Limitaciones detalladas

| Limitación | Explicación |
|---|---|
| **Escalabilidad horizontal** | Escalar añadiendo más servidores es complejo (vs. NoSQL que lo hace nativamente) |
| **Esquema rígido** | Cambiar la estructura de una tabla requiere migraciones (ALTER TABLE) |
| **Datos no estructurados** | JSON, documentos, imágenes no son el fuerte de las relacionales |
| **Costo en clusters** | En sistemas distribuidos grandes, mantener consistencia ACID es caro |

### Ejemplo de transacción ACID

```sql
-- Transferencia bancaria: ambas operaciones deben ocurrir o ninguna
START TRANSACTION;
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;
COMMIT;
-- Si algo falla antes del COMMIT, hacemos ROLLBACK y nada cambia
```

---

## Bases de datos SQL vs NoSQL

```mermaid
flowchart LR
    DB[Bases de Datos] --> SQL[SQL - Relacionales]
    DB --> NoSQL[NoSQL]

    SQL --> S1[MySQL, PostgreSQL, SQLite, Oracle, SQL Server]
    NoSQL --> N1["Documentales - MongoDB, Firestore"]
    NoSQL --> N2["Clave-Valor - Redis, DynamoDB"]
    NoSQL --> N3["Columnares - Cassandra, Bigtable"]
    NoSQL --> N4["Grafos - Neo4j, ArangoDB"]
```

### Comparativa directa

| Característica | SQL | NoSQL |
|---|---|---|
| **Modelo de datos** | Tablas, filas, columnas | Documentos, clave-valor, grafos, columnas |
| **Esquema** | Fijo (definido al crear la tabla) | Flexible (cada documento puede tener distintos campos) |
| **Escalabilidad** | Vertical (más potencia en un servidor) | Horizontal (más servidores) |
| **Transacciones** | ACID (fuerte consistencia) | BASE (disponibilidad y tolerancia partición) |
| **Consultas** | SQL potente con JOINs, subconsultas | Limitadas al modelo específico |
| **Casos de uso** | Finanzas, ERP, sistemas contables, apps tradicionales | Big data, redes sociales, IoT, tiempo real |
| **Ejemplos** | PostgreSQL, MySQL, Oracle | MongoDB, Redis, Cassandra, Neo4j |

### ¿Cuándo elegir cada uno?

```mermaid
flowchart TD
    P{¿Tus datos tienen\nrelaciones complejas?} -->|Sí| SQL[SQL]
    P -->|No| P2{¿Necesitas escalar\nhorizontalmente?}

    P2 -->|Sí| NoSQL[NoSQL]
    P2 -->|No| P3{¿Esquema fijo\no flexible?}

    P3 -->|Fijo| SQL
    P3 -->|Flexible| NoSQL

    SQL --> ConclusiónSQL["Ej: Sistema bancario, ERP, CRM"]
    NoSQL --> ConclusiónNoSQL["Ej: Red social, logs, catálogo productos"]
```

### Ejemplo práctico: mismo dato en SQL vs NoSQL

**En SQL (PostgreSQL):**
```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100),
    email VARCHAR(200),
    direccion VARCHAR(300),
    created_at TIMESTAMP DEFAULT NOW()
);

INSERT INTO usuarios (nombre, email, direccion)
VALUES ('Carlos', 'carlos@email.com', 'Calle 123');
```

**En NoSQL (MongoDB):**
```javascript
db.usuarios.insertOne({
    nombre: "Carlos",
    email: "carlos@email.com",
    direccion: "Calle 123",
    created_at: new Date(),
    // Podemos añadir campos distintos en cada documento
    telefono: "555-1234"  // Esto no afecta a otros documentos
});
```

### El mito de "SQL vs NoSQL" — convivencia

Hoy en día es común usar **ambos** en una misma aplicación (arquitectura **políglota**):

```mermaid
flowchart LR
    App[Aplicación] --> PG[(PostgreSQL\ndatos transaccionales)]
    App --> Mongo[(MongoDB\ncatálogo productos)]
    App --> Redis[(Redis\nsesiones y caché)]
```

Cada tecnología se usa para lo que mejor sabe hacer.

---

## Resumen visual del viaje

```mermaid
flowchart TD
    Inicio[📦 Datos sin organizar] --> Tablas[🗂️ Tablas con filas y columnas]
    Tablas --> Relaciones[🔗 Relaciones entre tablas\nPK y FK]
    Relaciones --> SQL[💬 Consultas con SQL\nSELECT, INSERT, UPDATE, DELETE]
    SQL --> ACID[🛡️ Transacciones ACID]
    ACID --> Decision{🤔 ¿Suficiente\nescalabilidad?}
    Decision -->|Sí| Quedarse[✅ RDBMS clásico\nPostgreSQL / MySQL]
    Decision -->|No| Hibrido[🔄 Arquitectura híbrida\nSQL + NoSQL]
```

## Recursos para practicar

- [SQLite Tutorial](https://www.sqlitetutorial.net/) — Base de datos ligera para practicar localmente
- [DB Fiddle](https://www.db-fiddle.com/) — Prueba SQL online sin instalar nada
- [PostgreSQL Exercises](https://pgexercises.com/) — Ejercicios prácticos de SQL

## Relacionados:
- [[sintaxis-basica-sql]] #siguiente 