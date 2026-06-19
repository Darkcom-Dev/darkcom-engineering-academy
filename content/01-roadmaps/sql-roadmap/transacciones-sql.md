# Transacciones

Una **transacción** es un conjunto de operaciones SQL que se ejecutan como una **unidad lógica de trabajo**. O se ejecutan **todas** o **ninguna**.

```mermaid
flowchart TD
    INICIO["🔄 BEGIN TRANSACTION"] --> OP1["UPDATE cuenta_origen - $100"]
    OP1 --> OP2["UPDATE cuenta_destino + $100"]
    OP2 --> DECISION{"¿Todo correcto?"}
    DECISION -->|Sí| COMMIT["✅ COMMIT Cambios permanentes"]
    DECISION -->|No| ROLLBACK["❌ ROLLBACK Todo vuelve al inicio"]
```

### Ejemplo clásico: transferencia bancaria

```sql
-- Sin transacción: si falla la segunda operación, el dinero se pierde
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;  -- ❌ Si esto falla...

-- Con transacción: ambas se ejecutan o ninguna
START TRANSACTION;
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;
COMMIT;
-- Si algo falla antes de COMMIT, hacemos ROLLBACK y todo vuelve como estaba
```

---

## BEGIN

Inicia una transacción explícita.

```mermaid
flowchart LR
    INICIO["Punto de inicio"] --> BEGIN["BEGIN / START TRANSACTION"]
    BEGIN --> OPERACIONES["Operaciones SQL"]
```

### Sintaxis

```sql
-- MySQL / MariaDB
START TRANSACTION;

-- PostgreSQL
BEGIN;

-- SQL Server
BEGIN TRANSACTION;

-- MySQL también acepta
BEGIN;
START TRANSACTION;  -- equivalente
```

```sql
START TRANSACTION;

INSERT INTO pedidos (cliente_id, total) VALUES (1, 500.00);
UPDATE productos SET stock = stock - 1 WHERE id = 42;
DELETE FROM carrito_compras WHERE usuario_id = 1;

-- Todo esto está en una "burbuja" que podemos deshacer
```

### Comportamiento por defecto

```sql
-- MySQL: autocommit = ON (cada statement es su propia transacción)
SHOW VARIABLES LIKE 'autocommit';
SET autocommit = 0;  -- Desactivar autocommit

-- PostgreSQL: autocommit = ON por defecto
-- SQL Server: autocommit = ON por defecto

-- Con autocommit ON:
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
-- ↑ Esto se confirma automáticamente

-- Con BEGIN / START TRANSACTION, autocommit se desactiva hasta COMMIT/ROLLBACK
START TRANSACTION;
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
-- ↑ Esto NO se ha confirmado todavía
COMMIT;  -- Ahora se confirma
```

---

## COMMIT

Confirma **permanentemente** todas las operaciones realizadas desde el BEGIN.

```mermaid
flowchart LR
    TRANS["Transacción en curso"] --> COMMIT["COMMIT"]
    COMMIT --> PERMANENTE["✅ Cambios guardados permanentemente en DB"]
    COMMIT --> FIN["Fin de transacción"]
```

### Sintaxis

```sql
COMMIT;
-- o
COMMIT WORK;  -- equivalente
```

```sql
START TRANSACTION;

UPDATE cuentas SET saldo = saldo - 500 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 500 WHERE id = 2;

-- Si llegamos aquí sin errores:
COMMIT;
-- Los cambios son ahora visibles para otras conexiones
-- y persistentes aunque el servidor se apague
```

### ¿Qué pasa si hay un error?

```sql
START TRANSACTION;

UPDATE cuentas SET saldo = saldo - 500 WHERE id = 1;
-- ¡Error! La cuenta 2 no existe
UPDATE cuentas SET saldo = saldo + 500 WHERE id = 999;
-- ERROR: Foreign key violation

-- Sin COMMIT explícito:
-- ⚠️ La primera operación NO se ha confirmado
-- ⚠️ La transacción sigue abierta (puede bloquear otras operaciones)

-- MySQL: autocommit no libera los locks hasta COMMIT o ROLLBACK
```

> ⚡ **Siempre haz COMMIT o ROLLBACK explícitamente.** Las transacciones abiertas mantienen locks y pueden causar deadlocks.

---

## ROLLBACK

Deshace **todas** las operaciones realizadas desde el BEGIN (o desde el último SAVEPOINT).

```mermaid
flowchart LR
    ERROR["💥 Ocurre un error"] --> ROLLBACK["ROLLBACK"]
    ROLLBACK --> DESHACER["↩️ Todo vuelve al estado anterior al BEGIN"]
    ROLLBACK --> FIN["Fin de transacción (sin cambios)"]
```

### Sintaxis

```sql
ROLLBACK;
-- o
ROLLBACK WORK;  -- equivalente
```

```sql
START TRANSACTION;

-- Intentar transferencia
UPDATE cuentas SET saldo = saldo - 500 WHERE id = 1;

-- Verificar saldo después del descuento
SELECT saldo FROM cuentas WHERE id = 1;
-- Saldo: 500 (era 1000)

-- Algo sale mal con la cuenta destino
-- Deshacemos todo:
ROLLBACK;

-- Verificar que el saldo volvió a su valor original
SELECT saldo FROM cuentas WHERE id = 1;
-- Saldo: 1000 ✅ (se deshizo el UPDATE)
```

### Ejemplo con control de errores

```sql
-- MySQL: con señales
START TRANSACTION;

UPDATE cuentas SET saldo = saldo - 500 WHERE id = 1;

IF (SELECT saldo FROM cuentas WHERE id = 1) < 0 THEN
    ROLLBACK;
    SELECT 'Saldo insuficiente' AS error;
ELSE
    UPDATE cuentas SET saldo = saldo + 500 WHERE id = 2;
    COMMIT;
END IF;
```

---

## SAVEPOINT

Crea un **punto de control** dentro de una transacción. Permite deshacer solo una parte de la transacción sin cancelarla completamente.

```mermaid
flowchart TD
    INICIO["BEGIN"] --> SAVEPOINT_A["SAVEPOINT sp1"]
    SAVEPOINT_A --> OP1["UPDATE cuenta 1"]
    OP1 --> SAVEPOINT_B["SAVEPOINT sp2"]
    SAVEPOINT_B --> OP2["UPDATE cuenta 2"]
    OP2 --> ERROR["💥 Error en UPDATE cuenta 2"]
    ERROR --> ROLLBACK_TO["ROLLBACK TO sp2"]
    ROLLBACK_TO --> OP1_MOD["UPDATE cuenta 2 corregido"]
    OP1_MOD --> COMMIT["✅ COMMIT cuenta 1 actualizada cuenta 2 actualizada"]

    ERROR --> ROLLBACK_TO2["ROLLBACK TO sp1"]
    ROLLBACK_TO2 --> OTRAS_OPS["Otras operaciones..."]
    OTRAS_OPS --> COMMIT2["✅ COMMIT (solo cuenta 1 actualizada)"]
```

### Sintaxis

```sql
SAVEPOINT nombre_punto;
ROLLBACK TO SAVEPOINT nombre_punto;
RELEASE SAVEPOINT nombre_punto;  -- elimina el savepoint
```

### Ejemplo práctico

```sql
START TRANSACTION;

INSERT INTO logs(mensaje) VALUES('Iniciando proceso de pago');
SAVEPOINT log_creado;

-- Descontar del primer producto
UPDATE inventario SET stock = stock - 1 WHERE id = 100;
SAVEPOINT inventario_descontado;

-- Cobrar al cliente
UPDATE cuentas SET saldo = saldo - 500 WHERE id = 1;

-- Si el cobro falla (saldo insuficiente), podemos:
-- 1. Deshacer solo el cobro y el descuento (parcial)
ROLLBACK TO SAVEPOINT inventario_descontado;
-- Ahora podemos intentar otro método de pago
UPDATE cuentas SET saldo = saldo - 500 WHERE id = 2;  -- otra cuenta
COMMIT;  -- Confirma el log y el descuento del inventario

-- 2. O deshacer todo excepto el log
ROLLBACK TO SAVEPOINT log_creado;
COMMIT;  -- Solo se guarda el log
```

### Liberar savepoints

```sql
START TRANSACTION;

SAVEPOINT sp1;
INSERT INTO ...
SAVEPOINT sp2;
INSERT INTO ...

-- Ya no necesitamos sp1 (podemos liberarlo)
RELEASE SAVEPOINT sp1;

-- ROLLBACK a sp2 aún funciona
ROLLBACK TO sp2;

COMMIT;
```

---

## ACID

Las propiedades **ACID** garantizan que las transacciones se procesan de forma confiable.

```mermaid
flowchart TD
    ACID["🧪 PROPIEDADES ACID"] --> A["A - Atomicidad ✅ Todo o nada Si falla una operación, se deshacen todas"]
    ACID --> C["C - Consistencia ✅ Los datos siempre cumplen las reglas (restricciones, tipos)"]
    ACID --> I["I - Aislamiento ✅ Transacciones simultáneas no se interfieren entre sí"]
    ACID --> D["D - Durabilidad ✅ Una vez COMMIT, los datos persisten (incluso si hay fallo)"]
```

| Propiedad | Significado | Ejemplo |
|---|---|---|
| **A**tomicidad | La transacción es indivisible: se ejecuta completa o no se ejecuta | Transferencia: si falla el crédito, se deshace el débito |
| **C**onsistencia | Las reglas de la BD (PK, FK, CHECK) siempre se cumplen | No se puede insertar un pedido sin cliente |
| **I**solamiento | Las transacciones concurrentes no se ven entre sí hasta el COMMIT | Dos transferencias simultáneas no se mezclan |
| **D**urabilidad | Una vez COMMIT, los datos están seguros en disco | Si se corta la luz después del COMMIT, los datos persisten |

### Ejemplo ACID en acción

```sql
-- Transferencia bancaria: ACID completo
START TRANSACTION;

-- Atomicidad: ambas actualizaciones son una unidad
UPDATE cuentas SET saldo = saldo - 1000 WHERE id = 1;

-- Consistencia: CHECK saldo >= 0 se aplica (si existe)
-- Si saldo queda negativo, el motor rechaza y se debe hacer ROLLBACK

-- Aislamiento: otra transacción no ve el saldo reducido hasta COMMIT
UPDATE cuentas SET saldo = saldo + 1000 WHERE id = 2;

COMMIT;
-- Durabilidad: en este punto, los cambios están en disco
```

---

## Niveles de aislamiento de transacciones

Controlan cómo las transacciones concurrentes interactúan entre sí.

```mermaid
flowchart TD
    NIVELES[Niveles de aislamiento] --> READ_UNCOMMITTED["READ UNCOMMITTED 👻 Lectura sucia Lectura no repetible Fantasma"]
    NIVELES --> READ_COMMITTED["READ COMMITTED ✅ No lectura sucia 👻 Lectura no repetible Fantasma"]
    NIVELES --> REPEATABLE_READ["REPEATABLE READ ✅ No lectura sucia ✅ Lectura repetible 👻 Fantasma"]
    NIVELES --> SERIALIZABLE["SERIALIZABLE ✅ No lectura sucia ✅ Lectura repetible ✅ No fantasmas (más lento)"]

    READ_UNCOMMITTED --> MAS_RAPIDO["⚡ Más rápido"]
    SERIALIZABLE --> MAS_SEGURO["🛡️ Más seguro"]
```

### Problemas que resuelven los niveles de aislamiento

```mermaid
flowchart LR
    DIRTY["👻 Lectura Sucia Leer datos NO COMMITEADOS de otra transacción"] --> TIPO1["Ej: Transacción A actualiza, Transacción B lee el nuevo valor, A hace ROLLBACK → B tiene dato inválido"]
    NON_REPEATABLE["👻 Lectura No Repetible Misma consulta, dos resultados diferentes en misma transacción"] --> TIPO2["Ej: SELECT saldo da 1000, otra transacción lo cambia a 500, mismo SELECT da 500"]
    PHANTOM["👻 Lectura Fantasma Nuevas filas aparecen en consultas repetidas"] --> TIPO3["Ej: SELECT COUNT(*) da 5, otra transacción INSERTA 1 fila, misma consulta da 6"]
```

### 1. READ UNCOMMITTED

Nivel más bajo: una transacción puede leer datos **no commiteados** de otra.

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
-- En SQL Server
-- En MySQL: InnoDB usa REPEATABLE READ por defecto
-- En PostgreSQL: no permite READ UNCOMMITTED (lo trata como READ COMMITTED)
```

```mermaid
flowchart LR
    A["Transacción A UPDATE saldo = 500 (NO COMMIT aún)"] --> B["Transacción B SELECT saldo → 500 (lee dato no commiteado)"]
    A --> A_ROLLBACK["ROLLBACK (saldo vuelve a 1000)"]
    B --> B_ERROR["❌ Transacción B tiene 500 (dato inválido)"]
```

> ⚠️ **Riesgo alto:** Lectura sucia garantizada. Solo útil para estimaciones aproximadas.

### 2. READ COMMITTED (default en PostgreSQL, SQL Server, Oracle)

Cada consulta ve solo datos **commiteados**. Previene lectura sucia, pero permite lectura no repetible.

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

```mermaid
flowchart LR
    A["Transacción A UPDATE saldo = 500"] --> A_COMMIT["COMMIT"]
    B1["Transacción B SELECT saldo → 1000 (antes de COMMIT de A)"] --> B2["Transacción B SELECT saldo → 500 (después de COMMIT de A)"]
    B1 --> PROBLEMA["👻 Lectura no repetible: misma transacción dos resultados distintos"]
```

### 3. REPEATABLE READ (default en MySQL/InnoDB)

Garantiza que si lees un dato, **siempre verás el mismo valor** dentro de la transacción. Previene lectura sucia y no repetible, pero permite lecturas fantasma.

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

```mermaid
flowchart LR
    B1["Transacción B SELECT saldo → 1000"] --> B2["Transacción B SELECT saldo → 1000 (¡siempre el mismo!)"]
    A["Transacción A COMMIT con saldo = 500"] -.->|"B no ve el cambio"| B2
    B2 --> B3["Transacción A INSERT nueva fila"]
    B3 --> B4["Transacción B SELECT COUNT(*) → difiere"]
    B4 --> PROBLEMA["👻 Lectura fantasma: nuevas filas aparecen"]
```

### 4. SERIALIZABLE

Máximo aislamiento: las transacciones se ejecutan como si fueran **secuenciales**. Previene todos los problemas.

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

```mermaid
flowchart LR
    A["Transacción A Transferencia $100"] --> WAIT["⏳ Espera si B está activa en mismos datos"]
    B["Transacción B Transferencia $200"] --> FIN_B["B termina"]
    WAIT --> FIN_A["A ejecuta después"]
    FIN_B --> RESULTADO["✅ Resultado: serial como si fueran una tras otra"]
```

### Tabla comparativa

| Nivel | Lectura sucia | Lectura no repetible | Fantasmas | Rendimiento |
|---|---|---|---|---|
| **READ UNCOMMITTED** | ❌ Puede ocurrir | ❌ Puede ocurrir | ❌ Puede ocurrir | ⚡ Más rápido |
| **READ COMMITTED** | ✅ No | ❌ Puede ocurrir | ❌ Puede ocurrir | ⚡ Rápido |
| **REPEATABLE READ** | ✅ No | ✅ No | ❌ Puede ocurrir | 🐢 Más lento |
| **SERIALIZABLE** | ✅ No | ✅ No | ✅ No | 🐢 Más lento |

### Configurar el nivel de aislamiento

```sql
-- A nivel de sesión (solo para esta conexión)
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- A nivel global (para todas las nuevas conexiones)
SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Para una transacción específica
START TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### ¿Qué nivel deberías usar?

```mermaid
flowchart TD
    P["¿Qué nivel de aislamiento usar?"] --> P1["¿Tu app necesita máxima consistencia?"]
    P1 -->|Sí| P2["¿Aceptas menor rendimiento?"]
    P2 -->|Sí| SERIAL["SERIALIZABLE 💰 Finanzas, pagos, contabilidad"]
    P2 -->|No| RR["REPEATABLE READ 📊 Reportes que necesitan consistencia de lectura"]
    P1 -->|No| RC["READ COMMITTED 🌐 La mayoría de apps web (default PostgreSQL)"]
    P1 -->|Solo estimaciones| RU["READ UNCOMMITTED 📈 Estadísticas, dashboards aproximados"]
```

---

## Resumen visual

```mermaid
flowchart TD
    TRANS["🔄 Transacciones SQL"] --> BEGIN["BEGIN / START TRANSACTION Inicia la unidad de trabajo"]

    TRANS --> OPS["Operaciones SQL (INSERT, UPDATE, DELETE)"]

    TRANS --> DECISION{"¿Todo correcto?"}
    DECISION -->|Sí| COMMIT["COMMIT 🔒 Cambios permanentes"]
    DECISION -->|No| ROLLBACK["ROLLBACK ↩️ Todo deshecho"]

    TRANS --> SAVEPOINT["SAVEPOINT 🔖 Punto de control parcial"]
    SAVEPOINT --> SP_USO["ROLLBACK TO SAVEPOINT ↩️ Deshace hasta aquí (no cancela toda la transacción)"]

    TRANS --> ACID["🧪 ACID Atomicidad, Consistencia, Aislamiento, Durabilidad"]

    TRANS --> AISLAMIENTO["🔒 Niveles de aislamiento"]
    AISLAMIENTO --> RU2["READ UNCOMMITTED Menos seguro, más rápido"]
    AISLAMIENTO --> RC2["READ COMMITTED Default PostgreSQL"]
    AISLAMIENTO --> RR2["REPEATABLE READ Default MySQL"]
    AISLAMIENTO --> SERIAL2["SERIALIZABLE Más seguro, más lento"]
```

### Guía rápida

| Comando | ¿Qué hace? |
|---|---|
| `START TRANSACTION` | Inicia una transacción explícita |
| `COMMIT` | Guarda permanentemente los cambios |
| `ROLLBACK` | Deshace todos los cambios de la transacción |
| `SAVEPOINT nombre` | Crea un punto de control dentro de la transacción |
| `ROLLBACK TO nombre` | Deshace hasta el savepoint |
| `RELEASE SAVEPOINT nombre` | Elimina un savepoint |
| `SET TRANSACTION ISOLATION LEVEL ...` | Cambia el nivel de aislamiento |
## Relacionados:
- [[indices-sql]] #anterior 
- [[integridad-y-seguridad-de-los-datos-sql]] #siguiente 