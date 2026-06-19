# Integridad y seguridad de los datos

La **integridad** garantiza que los datos sean correctos y consistentes. La **seguridad** controla quién puede acceder a ellos y qué puede hacer.

```mermaid
flowchart TD
    TITLE[Integridad y Seguridad] --> INTEGRIDAD["🛡️ Integridad Los datos son correctos, consistentes y válidos"]
    TITLE --> SEGURIDAD["🔐 Seguridad Solo usuarios autorizados pueden acceder/modificar"]

    INTEGRIDAD --> RESTRICCIONES["Restricciones PK, FK, UNIQUE, CHECK, NOT NULL"]
    INTEGRIDAD --> TRANSACCIONES["Transacciones ACID"]
    INTEGRIDAD --> TRIGGERS["Triggers Validaciones automáticas"]

    SEGURIDAD --> AUTENTICACION["Autenticación ¿Quién eres?"]
    SEGURIDAD --> AUTORIZACION["Autorización ¿Qué puedes hacer?"]
    SEGURIDAD --> AUDITORIA["Auditoría Registro de actividades"]
```

---

## Restricciones de integridad de datos

Son reglas que la base de datos aplica automáticamente para garantizar que los datos sean válidos.

```mermaid
flowchart TD
    RESTRICCIONES[Restricciones de integridad] --> INTEGRIDAD_ENTIDAD["Integridad de Entidad Cada fila debe ser única → PRIMARY KEY"]
    RESTRICCIONES --> INTEGRIDAD_REFERENCIAL["Integridad Referencial Las relaciones deben ser consistentes → FOREIGN KEY"]
    RESTRICCIONES --> INTEGRIDAD_DOMINIO["Integridad de Dominio Los valores deben cumplir el tipo y reglas → CHECK, NOT NULL, DEFAULT"]
    RESTRICCIONES --> INTEGRIDAD_UNICA["Integridad Única No puede haber duplicados → UNIQUE"]
```

### Integridad de entidad (PRIMARY KEY)

Cada fila debe tener un identificador único. Garantiza que no hay filas duplicadas.

```sql
-- ✅ Cada empleado tiene un ID único
CREATE TABLE empleados (
    id INT PRIMARY KEY,  -- No puede haber dos empleados con el mismo ID
    nombre VARCHAR(100)
);

-- ❌ Error: no se puede insertar duplicado
INSERT INTO empleados VALUES (1, 'Ana');
INSERT INTO empleados VALUES (1, 'Carlos');  -- Error: duplicate entry '1'
```

### Integridad referencial (FOREIGN KEY)

Las relaciones entre tablas deben ser consistentes. No puede haber un hijo sin padre.

```sql
-- ✅ Todo pedido debe pertenecer a un cliente existente
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nombre VARCHAR(100)
);

CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    cliente_id INT,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

-- ❌ Error: no se puede crear un pedido para un cliente que no existe
INSERT INTO pedidos VALUES (1, 999);  -- Error: foreign key constraint fails
```

### Integridad de dominio (CHECK, NOT NULL, DEFAULT)

Los valores deben cumplir las reglas definidas para cada columna.

```sql
CREATE TABLE productos (
    id INT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,        -- No puede ser NULL
    precio DECIMAL(10,2) CHECK (precio > 0),  -- Debe ser positivo
    stock INT DEFAULT 0,                 -- Valor por defecto
    categoria VARCHAR(20) CHECK (categoria IN ('A', 'B', 'C'))  -- Valores permitidos
);

-- ✅ Válido
INSERT INTO productos VALUES (1, 'Laptop', 15000.00, 10, 'A');

-- ❌ Error: precio negativo
INSERT INTO productos VALUES (2, 'Mouse', -100.00, 5, 'A');  -- CHECK failed

-- ❌ Error: categoría inválida
INSERT INTO productos VALUES (3, 'Teclado', 500.00, 3, 'Z');  -- CHECK failed

-- ❌ Error: nombre NULL
INSERT INTO productos VALUES (4, NULL, 100.00, 2, 'B');  -- NOT NULL failed
```

### Integridad única (UNIQUE)

Garantiza que no haya valores duplicados en una columna o combinación de columnas.

```sql
-- ✅ No puede haber dos usuarios con el mismo email
CREATE TABLE usuarios (
    id INT PRIMARY KEY,
    email VARCHAR(200) UNIQUE,
    curp VARCHAR(18) UNIQUE
);

-- ❌ Error: email duplicado
INSERT INTO usuarios VALUES (1, 'ana@mail.com', 'CURP123');
INSERT INTO usuarios VALUES (2, 'ana@mail.com', 'CURP456');  -- Error: duplicate
```

---

## GRANT y REVOKE

Controlan los **permisos** de usuarios en la base de datos (DCL - Data Control Language).

### GRANT: otorgar permisos

```mermaid
flowchart LR
    ADMIN["👤 DBA / Admin"] --> GRANT["GRANT permiso TO usuario"]
    GRANT --> OBJETO["ON objeto tabla, vista, BD"]
    GRANT --> USUARIO["Usuario o rol destinatario"]
```

```sql
-- Sintaxis básica
GRANT privilegio ON objeto TO usuario;

-- Otorgar SELECT en una tabla
GRANT SELECT ON empleados TO usuario_lectura;

-- Otorgar múltiples permisos
GRANT SELECT, INSERT, UPDATE ON empleados TO usuario_editor;

-- Otorgar todos los permisos
GRANT ALL PRIVILEGES ON empleados TO usuario_admin;

-- Otorgar a nivel de base de datos
GRANT SELECT ON mi_base_de_datos.* TO usuario_lectura;

-- Otorgar con opción de transferir permisos
GRANT SELECT ON empleados TO usuario_admin WITH GRANT OPTION;
-- usuario_admin puede dar permiso SELECT a otros usuarios
```

### Tipos de privilegios

| Nivel | Privilegios comunes |
|---|---|
| **Tabla** | `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `ALTER`, `REFERENCES`, `ALL` |
| **Base de datos** | `CREATE`, `DROP`, `ALTER`, `CREATE VIEW`, `CREATE ROUTINE` |
| **Global** | `SUPER`, `RELOAD`, `SHUTDOWN`, `PROCESS`, `FILE`, `ALL` |

```sql
-- Privilegios a nivel de columna (MySQL, PostgreSQL)
GRANT SELECT (nombre, email) ON empleados TO usuario_lectura_parcial;
-- Solo puede ver nombre y email, no puede ver salario

-- Privilegios a nivel de tabla
GRANT SELECT ON empleados TO 'analista'@'localhost';

-- PostgreSQL: crear privilegios específicos
GRANT SELECT, INSERT ON empleados TO analista;
GRANT USAGE ON SCHEMA public TO analista;
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO analista;
```

### REVOKE: revocar permisos

```mermaid
flowchart LR
    ADMIN["👤 DBA / Admin"] --> REVOKE["REVOKE permiso FROM usuario"]
    REVOKE --> USUARIO["Usuario o rol destinatario"]
    USUARIO --> PERDIDA["❌ Permiso eliminado"]
```

```sql
-- Sintaxis básica
REVOKE privilegio ON objeto FROM usuario;

-- Revocar SELECT
REVOKE SELECT ON empleados FROM usuario_lectura;

-- Revocar múltiples permisos
REVOKE INSERT, UPDATE, DELETE ON empleados FROM usuario_editor;

-- Revocar todos los permisos
REVOKE ALL PRIVILEGES ON empleados FROM usuario_admin;

-- Revocar con CASCADE (también revoca permisos que dio este usuario)
REVOKE SELECT ON empleados FROM usuario_admin CASCADE;
```

### Roles: gestionar permisos por grupos

```mermaid
flowchart TD
    ROLES["Roles"] --> ROL_LECTURA["ROL_lectura SELECT en todas las tablas"]
    ROLES --> ROL_EDITOR["ROL_editor SELECT, INSERT, UPDATE"]
    ROLES --> ROL_ADMIN["ROL_admin Todos los privilegios"]

    ROL_LECTURA --> USUARIO1["Usuario: analista"]
    ROL_LECTURA --> USUARIO2["Usuario: lector_externo"]
    ROL_EDITOR --> USUARIO3["Usuario: editor_contenido"]
    ROL_ADMIN --> USUARIO4["Usuario: dba"]
```

```sql
-- PostgreSQL: crear roles
CREATE ROLE rol_lectura;
CREATE ROLE rol_editor;

-- Asignar permisos al rol
GRANT SELECT ON ALL TABLES IN SCHEMA public TO rol_lectura;
GRANT SELECT, INSERT, UPDATE ON ALL TABLES IN SCHEMA public TO rol_editor;

-- Asignar rol a usuarios
GRANT rol_lectura TO analista;
GRANT rol_editor TO editor_contenido;

-- MySQL: roles (desde MySQL 8)
CREATE ROLE 'rol_lectura', 'rol_editor';
GRANT SELECT ON mi_bd.* TO 'rol_lectura';
GRANT SELECT, INSERT, UPDATE ON mi_bd.* TO 'rol_editor';
GRANT 'rol_lectura' TO 'analista'@'localhost';
```

### Verificar permisos

```sql
-- MySQL: ver permisos de un usuario
SHOW GRANTS FOR 'usuario'@'localhost';
SHOW GRANTS FOR CURRENT_USER;

-- PostgreSQL
SELECT * FROM information_schema.table_privileges
WHERE grantee = 'usuario';

-- Listar usuarios (MySQL)
SELECT user, host FROM mysql.user;
```

### Ejemplo completo: crear usuario con permisos

```sql
-- 1. Crear usuario (MySQL)
CREATE USER 'lector'@'localhost' IDENTIFIED BY 'contraseña_segura';

-- 2. Otorgar permisos específicos
GRANT SELECT ON empleados TO 'lector'@'localhost';
GRANT SELECT ON departamentos TO 'lector'@'localhost';

-- 3. Verificar
SHOW GRANTS FOR 'lector'@'localhost';

-- 4. Revocar si es necesario
REVOKE SELECT ON empleados FROM 'lector'@'localhost';

-- 5. Eliminar usuario
DROP USER 'lector'@'localhost';
```

---

## Mejores prácticas para la seguridad de BD

```mermaid
flowchart TD
    SEGURIDAD[Seguridad en BD] --> PRINCIPIO["🔑 Principio de Mínimo Privilegio Solo los permisos necesarios solo por el tiempo necesario"]
    SEGURIDAD --> AUTENTICACION2["🔐 Autenticación Fuerte Contraseñas seguras Autenticación multifactor"]
    SEGURIDAD --> ENCRIPTACION["🔒 Encriptación Datos en reposo y en tránsito SSL/TLS"]
    SEGURIDAD --> AUDITORIA2["📋 Auditoría Registrar intentos de acceso Monitorear actividad sospechosa"]
    SEGURIDAD --> BACKUP["💾 Backup y Recuperación Respaldos regulares Pruebas de restauración"]
    SEGURIDAD --> ACTUALIZACION["🔄 Mantenimiento Parches de seguridad Versiones actualizadas"]
```

### 1. Principio de mínimo privilegio

```sql
-- ❌ MAL: otorgar todos los permisos a todos
GRANT ALL PRIVILEGES ON *.* TO 'app'@'%';

-- ✅ BIEN: solo los permisos necesarios
CREATE USER 'app_web'@'localhost' IDENTIFIED BY 'contraseña_segura';
GRANT SELECT, INSERT, UPDATE ON mi_bd.pedidos TO 'app_web'@'localhost';
GRANT SELECT ON mi_bd.productos TO 'app_web'@'localhost';
-- No necesita DELETE ni ALTER ni DROP
```

### 2. Gestión de contraseñas

```sql
-- ✅ Contraseñas fuertes (mínimo 12 caracteres, combinación)
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'Kd9#mP2$xL7@qR5';

-- ✅ Cambiar contraseña regularmente
ALTER USER 'usuario'@'localhost' IDENTIFIED BY 'nueva_contraseña';

-- ✅ Expirar contraseñas (MySQL)
ALTER USER 'usuario'@'localhost' PASSWORD EXPIRE INTERVAL 90 DAY;

-- ❌ Contraseñas débiles
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'admin123';  -- ❌
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password';   -- ❌
```

### 3. Restringir acceso por host

```sql
-- ✅ Solo desde localhost (aplicación en mismo servidor)
CREATE USER 'app'@'localhost' IDENTIFIED BY 'segura';

-- ✅ Solo desde IP específica del servidor de aplicaciones
CREATE USER 'app'@'192.168.1.100' IDENTIFIED BY 'segura';

-- ✅ Desde cualquier IP de la red interna
CREATE USER 'app'@'192.168.1.%' IDENTIFIED BY 'segura';

-- ❌ Desde cualquier lugar (riesgo de seguridad)
CREATE USER 'app'@'%' IDENTIFIED BY 'segura';  -- ❌ Evitar si es posible
```

### 4. Encriptación

```sql
-- ✅ Encriptar datos sensibles a nivel de aplicación (nunca texto plano)
-- (MySQL / PostgreSQL: funciones de hash)
INSERT INTO usuarios (nombre, password_hash)
VALUES ('Ana', SHA2('contraseña', 256));  -- Hash, no texto plano

-- ✅ Conexiones SSL/TLS (MySQL)
ALTER USER 'usuario'@'localhost' REQUIRE SSL;

-- ✅ Encriptación a nivel de columna (PostgreSQL: pgcrypto)
CREATE EXTENSION pgcrypto;
UPDATE tarjetas SET numero_criptado = encrypt(numero, 'clave', 'aes');
```

### 5. Auditoría

```sql
-- MySQL: habilitar log general
SET GLOBAL general_log = 'ON';

-- MySQL Enterprise: auditoría
SELECT * FROM mysql.audit_log;

-- PostgreSQL: configuración de auditoría
-- En postgresql.conf:
-- logging_collector = on
-- log_statement = 'all'  (o 'ddl', 'mod')

-- PostgreSQL: pgaudit extension
CREATE EXTENSION pgaudit;

-- Ver conexiones activas (útil para detectar actividad sospechosa)
-- MySQL
SHOW PROCESSLIST;
SELECT * FROM information_schema.PROCESSLIST;

-- PostgreSQL
SELECT * FROM pg_stat_activity;
```

### 6. Respaldo y recuperación

```sql
-- MySQL: mysqldump
-- mysqldump -u root -p mi_bd > backup_$(date +%Y%m%d).sql

-- PostgreSQL: pg_dump
-- pg_dump -U postgres mi_bd > backup_$(date +%Y%m%d).sql

-- Restaurar
-- mysql -u root -p mi_bd < backup_20240101.sql
-- psql -U postgres mi_bd < backup_20240101.sql
```

### 7. Otras buenas prácticas

```sql
-- ❌ NO USAR: cuentas compartidas
-- Cada persona debe tener su propio usuario

-- ❌ NO USAR: contraseñas en código fuente
-- Usar variables de entorno o secretos

-- ✅ Deshabilitar comandos peligrosos para usuarios no admin
REVOKE FILE ON *.* FROM 'app_web'@'localhost';  -- No puede leer archivos
REVOKE SUPER ON *.* FROM 'app_web'@'localhost';  -- No puede hacer operaciones de admin
REVOKE DROP ON mi_bd.* FROM 'app_web'@'localhost';  -- No puede eliminar tablas

-- ✅ Revocar acceso a mysql.user (no puede ver contraseñas)
REVOKE SELECT ON mysql.* FROM 'app_web'@'localhost';

-- ✅ Configurar límites de recursos (MySQL)
ALTER USER 'app_web'@'localhost'
    WITH MAX_QUERIES_PER_HOUR 1000
    MAX_CONNECTIONS_PER_HOUR 100
    MAX_USER_CONNECTIONS 10;
```

### 8. SQL Injection: la amenaza #1

```sql
-- ❌ VULNERABLE: concatenar strings directamente
-- Código en la app:
-- query = "SELECT * FROM usuarios WHERE email = '" + email + "'";
-- Si email = "' OR '1'='1" → ¡lee TODOS los usuarios!

-- ✅ SEGURO: usar parámetros / prepared statements
-- En la app (PHP):
-- $stmt = $pdo->prepare("SELECT * FROM usuarios WHERE email = ?");
-- $stmt->execute([$email]);

-- ✅ En SQL mismo: escapar inputs (MySQL)
SELECT * FROM usuarios WHERE email = QUOTE('usuario@email.com');
```

```mermaid
flowchart LR
    APP["Aplicación"] --> QUERY["SELECT * FROM usuarios WHERE email = '$email'"]
    USUARIO["Atacante"] --> INYECCION["' OR '1'='1"]
    INYECCION --> QUERY
    QUERY --> RESULTADO["❌ ¡TODOS los usuarios! SELECT * FROM usuarios WHERE email = '' OR '1'='1'"]
```

---

## Resumen visual

```mermaid
flowchart TD
    DATOS["📦 Datos en BD"] --> INTEGRIDAD2["🛡️ Integridad"]
    DATOS --> SEGURIDAD2["🔐 Seguridad"]

    INTEGRIDAD2 --> ENTIDAD["Entidad PK"]
    INTEGRIDAD2 --> REFERENCIAL["Referencial FK"]
    INTEGRIDAD2 --> DOMINIO["Dominio CHECK, NOT NULL"]
    INTEGRIDAD2 --> UNICIDAD["Unicidad UNIQUE"]

    SEGURIDAD2 --> AUTENTICACION3["Autenticación Usuarios y contraseñas"]
    SEGURIDAD2 --> AUTORIZACION2["Autorización GRANT / REVOKE"]
    SEGURIDAD2 --> ROLES2["Roles Permisos por grupo"]
    SEGURIDAD2 --> MINIMO_PRIV["Mínimo privilegio Solo lo necesario"]
    SEGURIDAD2 --> ENCRIPTACION2["Encriptación SSL/TLS + Hashing"]
    SEGURIDAD2 --> AUDITORIA3["Auditoría Logs y monitoreo"]
```

### Checklist de seguridad

```mermaid
flowchart TD
    CHECKLIST[Lista de verificación] --> Q1["✅ ¿Cada usuario tiene SOLO los permisos que necesita?"]
    CHECKLIST --> Q2["✅ ¿Las contraseñas son seguras (>12 caracteres)?"]
    CHECKLIST --> Q3["✅ ¿Están restringidas las IPs que pueden conectarse?"]
    CHECKLIST --> Q4["✅ ¿Usas conexiones SSL/TLS?"]
    CHECKLIST --> Q5["✅ ¿Usas prepared statements contra SQL Injection?"]
    CHECKLIST --> Q6["✅ ¿Hay logs de actividad configurados?"]
    CHECKLIST --> Q7["✅ ¿Tienes backups automatizados y probados?"]
    CHECKLIST --> Q8["✅ ¿El servidor de BD está actualizado con parches?"]
```

## Relacionados:
- [[transacciones-sql]] #anterior 
- [[stored-procedures-and-functions-sql]] #siguiente 