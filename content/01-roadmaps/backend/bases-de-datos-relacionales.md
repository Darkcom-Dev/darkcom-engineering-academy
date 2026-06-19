# Bases de datos relacionales

Una **base de datos relacional** organiza los datos en **tablas** compuestas por filas y columnas. Las tablas se relacionan entre sí mediante claves (IDs), y se consultan con **SQL** (Structured Query Language).

```mermaid
flowchart TB
    subgraph Tablas ["🗂️ Ejemplo: Base de datos de un blog"]
        Usuarios["👤 usuarios<br/>id │ nombre │ email"]
        Posts["📝 posts<br/>id │ titulo │ contenido │ usuario_id"]
        Comentarios["💬 comentarios<br/>id │ texto │ post_id │ usuario_id"]
    end

    Usuarios -->|"1 → N<br/>un usuario crea muchos posts"| Posts
    Posts -->|"1 → N<br/>un post tiene muchos comentarios"| Comentarios
    Usuarios -->|"1 → N<br/>un usuario escribe muchos comentarios"| Comentarios

    style Usuarios fill:#bbdefb
    style Posts fill:#ffe0b2
    style Comentarios fill:#c8e6c9
```

### Conceptos clave

| Concepto      | Definición                                      |
| ------------- | ----------------------------------------------- |
| **Tabla**     | Conjunto de datos organizados en filas y columnas |
| **Fila**      | Un registro completo (ej. un usuario)             |
| **Columna**   | Un atributo del registro (ej. nombre, email)      |
| **PK**        | Primary Key — identifica únicamente cada fila     |
| **FK**        | Foreign Key — enlaza una fila con otra tabla      |
| **Índice**    | Acelera búsquedas en columnas específicas         |
| **Query**     | Consulta SQL para leer o modificar datos          |

### Tipos de relaciones

```mermaid
graph LR
    R1["1:1<br/>👤 usuario → 🆔 dni"]:::uno
    R2["1:N<br/>👤 usuario → 📝 posts"]:::unos
    R3["N:M<br/>📚 libros ↔ 👤 autores"]:::muchos

    classDef uno fill:#e3f2fd
    classDef unos fill:#fff3e0
    classDef muchos fill:#fce4ec
```

---

## SQL

SQL es el lenguaje universal para hablar con bases de datos relacionales. Se divide en sub-lenguajes:

```mermaid
flowchart LR
    SQL["📖 SQL"] --> DDL["🏗️ DDL<br/>CREATE, ALTER, DROP"]
    SQL --> DML["✏️ DML<br/>SELECT, INSERT, UPDATE, DELETE"]
    SQL --> DCL["🔐 DCL<br/>GRANT, REVOKE"]
    SQL --> TCL["🔄 TCL<br/>COMMIT, ROLLBACK"]

    style SQL fill:#e1f5fe
    style DDL fill:#ffe0b2
    style DML fill:#c8e6c9
    style DCL fill:#ffccbc
    style TCL fill:#e8eaf6
```

### Comandos DML esenciales

```sql
-- Leer datos
SELECT * FROM usuarios;
SELECT nombre, email FROM usuarios WHERE id = 1;
SELECT * FROM posts ORDER BY created_at DESC LIMIT 10;

-- Insertar datos
INSERT INTO usuarios (nombre, email) VALUES ('Ana', 'ana@email.com');

-- Actualizar datos
UPDATE usuarios SET email = 'nuevo@email.com' WHERE id = 1;

-- Eliminar datos
DELETE FROM usuarios WHERE id = 1;

-- JOINs (combinar tablas)
SELECT posts.titulo, usuarios.nombre
FROM posts
JOIN usuarios ON posts.usuario_id = usuarios.id;
```

### JOINs visuales

```mermaid
flowchart TB
    subgraph Joins ["🔗 Tipos de JOIN"]
        INNER["INNER JOIN<br/>❌ Solo filas que coinciden<br/>en ambas tablas"]
        LEFT["LEFT JOIN<br/>✅ Todas las filas de A<br/>❓ NULL si no hay match en B"]
        RIGHT["RIGHT JOIN<br/>✅ Todas las filas de B<br/>❓ NULL si no hay match en A"]
        FULL["FULL OUTER JOIN<br/>✅ Todas las filas de ambas<br/>❓ NULL donde no coinciden"]
    end

    style INNER fill:#c8e6c9
    style LEFT fill:#bbdefb
    style RIGHT fill:#ffe0b2
    style FULL fill:#f8bbd0
```

```
INNER JOIN:     ⚪───────⚪       LEFT JOIN:     ⚪───────⚪
                │  A ∩ B  │                     │  A  ∪ (A∩B)
                └─────────┘                     └─────────┘

RIGHT JOIN:    ⚪───────⚪       FULL JOIN:    ⚪───────⚪
               │ (A∩B) ∪ B │                 │  A  ∪  B  │
               └───────────┘                 └───────────┘
```

---

## SQLite

SQLite es una base de datos **embebida y sin servidor**. Todo el motor vive en un solo archivo `.db` o `.sqlite`. Es la más usada del mundo (cada smartphone tiene decenas de bases SQLite).

- **Sin servidor** — no necesitas instalar ni configurar nada
- **Zero configuración** — funciona con `sqlite3 database.db`
- **Ligera** — el binario pesa menos de 1 MB
- **Ideal para:** desarrollo local, prototipado, apps móviles, testing

```bash
# Crear y abrir una base de datos SQLite
sqlite3 mi_app.db

# Dentro de la consola SQLite:
sqlite> CREATE TABLE usuarios (id INTEGER PRIMARY KEY, nombre TEXT, email TEXT);
sqlite> INSERT INTO usuarios VALUES (1, 'Ana', 'ana@email.com');
sqlite> SELECT * FROM usuarios;
1|Ana|ana@email.com
sqlite> .tables        # listar tablas
sqlite> .schema        # ver esquema
sqlite> .exit          # salir
```

**¿Cuándo NO usar SQLite?** Cuando tienes múltiples escritores concurrentes (soporta escritura en serie), o cuando necesitas features avanzadas como roles de usuario, replicación o escalar horizontalmente.

---

## PostgreSQL

PostgreSQL (Postgres) es el base de datos relacional **open source más avanzado del mundo**. Es el estándar en la industria para aplicaciones web modernas.

- **Cliente-servidor** — corre como un servicio en un puerto (5432)
- **ACID compliant** — transacciones seguras y confiables
- **Tipos avanzados** — JSON, arrays, rangos, geometrías (GIS)
- **Extensiones** — PostGIS (geoespacial), pgvector (vectores), TimescaleDB (series temporales)
- **Ideal para:** producción, aplicaciones web, datos geoespaciales, analítica

```bash
# Conectarse a una base de datos PostgreSQL
psql -U usuario -d mi_app -h localhost

# Dentro de psql:
mi_app=# CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);

mi_app=# INSERT INTO usuarios (nombre, email)
         VALUES ('Ana', 'ana@email.com');

mi_app=# SELECT * FROM usuarios;
 id | nombre | email
----+--------+-----------------
  1 | Ana    | ana@email.com
(1 row)

mi_app=# \dt              # listar tablas
mi_app=# \d usuarios      # describir tabla
mi_app=# \q               # salir
```

### SQLite vs PostgreSQL: ¿cuándo usar cada uno?

```mermaid
flowchart TD
    P{🔍 ¿Para qué lo necesitas?}
    P -->|"Desarrollo local / testing"| SQ["📦 SQLite<br/>Rápido, sin setup"]
    P -->|"Prototipo / app pequeña"| SQ
    P -->|"App móvil / embebida"| SQ
    P -->|"Producción / app web"| PG["🐘 PostgreSQL<br/>Concurrencia, robustez"]
    P -->|"Datos geoespaciales"| PG
    P -->|"Múltiples escritores"| PG
    P -->|"Escalabilidad"| PG

    style SQ fill:#e8f5e9
    style PG fill:#e3f2fd
```

---

## Migraciones

Las **migraciones** son cambios versionados en el esquema de la base de datos. En lugar de modificar las tablas a mano en producción, escribes migraciones que se ejecutan en orden.

```mermaid
flowchart LR
    subgraph SinMigraciones ["🚫 Sin migraciones"]
        A1["✏️ Cambias la BD a mano"]
        A2["❌ Tus compañeros no tienen<br/>los mismos cambios"]
        A3["❌ Producción se rompe"]
    end

    subgraph ConMigraciones ["✅ Con migraciones"]
        B1["📝 Escribes migración V2"]
        B2["📤 Subes el código"]
        B3["🔄 Todos ejecutan<br/>migrate"]
        B4["✅ BD sincronizada"]
    end

    style SinMigraciones fill:#ffcdd2
    style ConMigraciones fill:#c8e6c9
```

### Ejemplo con Node.js (Knex.js)

```javascript
// migrations/20250101_create_users.js
exports.up = function(knex) {
    return knex.schema.createTable('usuarios', (table) => {
        table.increments('id');
        table.string('nombre').notNullable();
        table.string('email').unique().notNullable();
        table.timestamps(true, true);
    });
};

exports.down = function(knex) {
    return knex.schema.dropTable('usuarios');
};
```

```bash
# Comandos típicos
npx knex migrate:make create_users    # crear migración
npx knex migrate:latest                # aplicar migraciones pendientes
npx knex migrate:rollback              # deshacer la última migración
npx knex migrate:status                # ver estado de migraciones
```

### Ejemplo con Python (Alembic + SQLAlchemy)

```python
# migrations/versions/20250101_create_users.py
def upgrade():
    op.create_table(
        'usuarios',
        sa.Column('id', sa.Integer(), primary_key=True),
        sa.Column('nombre', sa.String(100), nullable=False),
        sa.Column('email', sa.String(255), nullable=False),
    )

def downgrade():
    op.drop_table('usuarios')
```

```bash
alembic revision --autogenerate -m "create users"
alembic upgrade head
```

### Buenas prácticas con migraciones

- **Una migración por cambio lógico** — no mezcles crear tabla + agregar columna en otra
- **Siempre define `down`** — para poder deshacer si algo sale mal
- **Nunca edites migraciones ya aplicadas** — crea una nueva
- **Versiona las migraciones en el repositorio** — así todo el equipo las tiene

---

## El problema de N+1

El **problema N+1** ocurre cuando haces una consulta que devuelve N registros, y luego haces 1 consulta adicional **por cada registro**. Resultado: 1 + N consultas donde bastaba con 1.

### Ejemplo del problema

```javascript
// ❌ N+1: 1 consulta para posts + N consultas para autores
const posts = db.query('SELECT * FROM posts');

posts.forEach(post => {
    const autor = db.query(                          // ← esto se ejecuta N veces
        'SELECT * FROM usuarios WHERE id = ?',
        [post.usuario_id]
    );
    console.log(post.titulo, autor.nombre);
});
```

```mermaid
flowchart TB
    subgraph NMasUno ["💥 N+1 (10 posts = 11 consultas)"]
        Q1["① 1 query: SELECT * FROM posts → 10 posts"]
        Q2["② 1 query: SELECT * FROM usuarios WHERE id=1"]
        Q3["③ 1 query: SELECT * FROM usuarios WHERE id=2"]
        Q4["..."]
        Q5["⑪ 1 query: SELECT * FROM usuarios WHERE id=10"]
    end

    subgraph Optimizado ["✅ Óptimo (10 posts = 2 consultas)"]
        O1["① 1 query: SELECT * FROM posts → 10 posts"]
        O2["② 1 query: SELECT * FROM usuarios WHERE id IN (1,2,3...10)"]
    end

    Q1 --> Q2 --> Q3 --> Q4 --> Q5
    O1 --> O2

    style NMasUno fill:#ffcdd2
    style Optimizado fill:#c8e6c9
```

### Cómo solucionarlo

```javascript
// ✅ Solución con JOIN (1 consulta)
const posts = db.query(`
    SELECT posts.titulo, usuarios.nombre AS autor
    FROM posts
    JOIN usuarios ON posts.usuario_id = usuarios.id
`);

// ✅ Solución con carga ansiosa (eager loading) en ORM
const posts = await Post.findAll({ include: Autor });
```

```sql
-- ✅ 1 sola consulta con JOIN
SELECT posts.titulo, usuarios.nombre AS autor
FROM posts
JOIN usuarios ON posts.usuario_id = usuarios.id
WHERE usuarios.activo = true;
```

### ¿Cómo detectar N+1?

1. **Log de consultas:** activa el logging de tu ORM y cuenta las queries
2. **Herramientas:**
   - `rails console` + gem `bullet` (Rails)
   - `django-debug-toolbar` (Django)
   - `knex-enable-logging` (Node.js)
3. **Síntoma:** una página tarda mucho cuando hay pocos registros, y el tiempo escala linealmente

```mermaid
graph LR
    Detectar["🔍 ¿Cómo saber si tengo N+1?"] --> Sintoma["⚠️ Síntoma:<br/>La página se vuelve<br/>lenta con pocos datos"]
    Detectar --> Logs["📊 Activa el log<br/>de consultas SQL"]
    Detectar --> Orm["🧰 Usa herramientas<br/>como Bullet o<br/>Debug Toolbar"]
    Logs --> Cuenta["🔢 Cuenta las queries:<br/>¿Ves el mismo SELECT<br/>una y otra vez?"]

    style Detectar fill:#e1f5fe
    style Sintoma fill:#fff3e0
    style Logs fill:#e8f5e9
    style Orm fill:#fce4ec
    style Cuenta fill:#ffcdd2
```

---

## Resumen visual

```mermaid
graph TB
    BD["🗄️ Bases de Datos Relacionales"] --> SQLite["📦 SQLite<br/>Dev / Testing / Móvil"]
    BD --> PostgreSQL["🐘 PostgreSQL<br/>Producción / Web"]

    BD --> SQL["📖 Lenguaje SQL"]
    SQL --> DDL["DDL: CREATE, ALTER, DROP"]
    SQL --> DML["DML: SELECT, INSERT, UPDATE, DELETE"]
    SQL --> Joins["JOIN: INNER, LEFT, RIGHT, FULL"]

    BD --> Migraciones["🔄 Migraciones<br/>Cambios versionados del esquema"]
    BD --> N1["💥 Problema N+1<br/>1 consulta + N consultas<br/>→ Solución: JOIN o eager loading"]

    style BD fill:#e1f5fe
    style SQLite fill:#e8f5e9
    style PostgreSQL fill:#e3f2fd
    style SQL fill:#fff3e0
    style Migraciones fill:#fce4ec
    style N1 fill:#ffcdd2
```

---

> **Siguiente paso:** Elige PostgreSQL para producción, usa SQLite para desarrollo y testing, y aprende un ORM (Prisma, Sequelize, SQLAlchemy) para trabajar más productivamente.

## Relacionados:
- [[servicio-de-hospedaje-de-repositorios]] #anterior 
- [[aprende-acerca-de-apis]] #siguiente 