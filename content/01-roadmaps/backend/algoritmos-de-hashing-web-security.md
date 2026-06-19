# Seguridad Web

La seguridad web protege los datos y sistemas contra accesos no autorizados, ataques y vulnerabilidades. Es responsabilidad de todo desarrollador entender los riesgos básicos y cómo mitigarlos.

```mermaid
flowchart TB
    SW["🛡️ Seguridad Web"] --> Hashing["🔐 Hashing<br/>Proteger contraseñas"]
    SW --> HTTPS["🔒 HTTPS / TLS<br/>Comunicación segura"]
    SW --> Headers["📋 Headers de seguridad<br/>CSP, HSTS, X-Frame-Options"]
    SW --> OWASP["⚠️ OWASP Top 10<br/>XSS, SQLi, CSRF, etc."]

    style SW fill:#e1f5fe
    style Hashing fill:#fff3e0
    style HTTPS fill:#c8e6c9
    style Headers fill:#fce4ec
    style OWASP fill:#f3e5f5
```

---

## Algoritmos de Hashing

El **hashing** convierte cualquier entrada en una cadena de longitud fija (hash) que es **irreversible**: no se puede obtener el valor original a partir del hash.

```mermaid
flowchart LR
    Input["🔤 Contraseña<br/>'MiPassword123'"] --> HashFunc["⚙️ Función Hash"]
    HashFunc --> Output["🔐 Hash (siempre igual)<br/>'a1b2c3d4e5f6...'"]
    Output --> Verificar["¿El hash coincide?<br/>Sí → contraseña correcta"]
    Input2["🔤 Intento<br/>'MiPassword124'"] --> HashFunc2["⚙️ Función Hash"]
    HashFunc2 --> Output2["🔐 Hash diferente<br/>'f9e8d7c6b5a4...'"]
    Output2 --> Verificar2["❌ No coincide"]

    style Input fill:#e8f5e9
    style Output fill:#c8e6c9
    style Input2 fill:#ffcdd2
    style Output2 fill:#ffcdd2
```

### Hashing vs Encriptación

```mermaid
flowchart LR
    subgraph Hashing_vs ["⚖️ Hashing vs Encriptación"]

        Hashing2["🔐 Hashing<br/>Irreversible<br/>Misma entrada = mismo hash<br/>Uso: contraseñas"]
        Encriptacion["🔑 Encriptación<br/>Reversible (con clave)<br/>Uso: datos sensibles<br/>(tarjetas, mensajes)"]
    end

    A["abc123"] -->|"hash"| H["a1b2c3..."]
    A -->|"encriptar"| E["xYz9..."]
    E -->|"desencriptar"| A2["abc123"]

    style Hashing_vs fill:#f5f5f5
    style Hashing2 fill:#fff3e0
    style Encriptacion fill:#e3f2fd
```

> **Regla:** Las contraseñas se **hashean** (nunca se encriptan). Si puedes desencriptar, es encriptación, no hash.

### Tipos de algoritmos

```mermaid
graph TB
    Algos["🔐 Algoritmos de Hash"] --> Rapidos["⚡ Rápidos (NO para contraseñas)<br/>MD5, SHA-1, SHA-256"]
    Algos --> Lentos["🐢 Lentos (SÍ para contraseñas)<br/>bcrypt, scrypt, argon2"]

    Rapidos --> Problema["❌ Fuerza bruta: pueden probar<br/>MILLONES de hashes/segundo"]
    Lentos --> Ventaja["✅ Fuerza bruta: lentos por diseño<br/>factor de trabajo ajustable"]

    style Algos fill:#e1f5fe
    style Rapidos fill:#ffcdd2
    style Lentos fill:#c8e6c9
    style Problema fill:#ffcdd2
    style Ventaja fill:#c8e6c9
```

---

## MD5

**MD5** (Message Digest 5) produce un hash de **128 bits** (32 caracteres hex). Fue popular en los 90, pero hoy está **completamente roto** para seguridad.

```bash
echo -n "MiPassword123" | md5sum
# → 4d9f5c6b8a3e2f1d7c0b9a8e7f6d5c4b
```

### Por qué NO usar MD5

1. **Colisiones demostradas** — se pueden generar dos entradas distintas con el mismo hash (2004)
2. **Rápidísimo** — se pueden calcular **miles de millones** de hashes por segundo con GPUs
3. **Sin sal (salt)** — mismo password = mismo hash (ataques con rainbow tables)

```mermaid
flowchart LR
    Pass["'password'"] --> MD5a["MD5: 5f4dcc3b5aa765d61d8327deb882cf99"]
    Pass2["'password'"] --> MD5b["MD5: 5f4dcc3b5aa765d61d8327deb882cf99"]
    MD5a -->|"⚠️ Mismo hash → rainbow table<br/>revela la contraseña"| Problem["❌ Inseguro"]

    style Pass fill:#ffcdd2
    style MD5a fill:#ffcdd2
    style MD5b fill:#ffcdd2
    style Problem fill:#ffcdd2
```

> **NO uses MD5 para contraseñas.** Solo sirve para checksums de integridad (verificar que un archivo no se corrompió).

---

## SHA

**SHA** (Secure Hash Algorithm) es una familia de funciones hash diseñadas por la NSA. Incluye SHA-1, SHA-2 (SHA-256, SHA-512) y SHA-3.

```mermaid
graph TB
    SHA["🔐 Familia SHA"] --> SHA1["SHA-1<br/>160 bits<br/>❌ Roto (colisiones 2017)"]
    SHA --> SHA2["SHA-2<br/>SHA-256 / SHA-512<br/>✅ Seguro<br/>Estándar actual"]
    SHA --> SHA3["SHA-3<br/>Último estándar<br/>✅ Seguro"]

    style SHA fill:#e1f5fe
    style SHA1 fill:#ffcdd2
    style SHA2 fill:#c8e6c9
    style SHA3 fill:#c8e6c9
```

### ¿SHA es seguro para contraseñas?

```bash
# SHA-256 de "MiPassword123"
echo -n "MiPassword123" | sha256sum
# → 8d9f7c6b5a4e3f2d1c0b9a8e7f6d5c4b3a2e1f0d9c8b7a6e5f4d3c2b1a0f9e8d
```

**SHA-256 es seguro criptográficamente, pero es DEMASIADO RÁPIDO** para contraseñas.

| Algoritmo     | Velocidad (hashes/seg en GPU) |
| ------------- | ----------------------------- |
| MD5           | ~20,000 millones              |
| SHA-1         | ~10,000 millones              |
| SHA-256       | ~5,000 millones               |
| SHA-512       | ~2,000 millones               |

> **¿Para qué sirve SHA entonces?** Firma digital, certificados SSL/TLS, integridad de archivos, Git (SHA-1 para commits), blockchain. **No para almacenar contraseñas.**

---

## Funciones de derivación de clave (KDF)

Para contraseñas necesitas algoritmos diseñados para ser **lentos y costosos** en CPU/memoria.

```mermaid
flowchart LR
    Passwd["🔤 Contraseña"] --> Salt["🧂 Salt aleatorio"]
    Salt --> KDF["🐢 KDF (Key Derivation Function)<br/>bcrypt / scrypt / argon2"]
    KDF --> Cost["⚙️ Factor de costo<br/>(más alto = más lento)"]
    Cost --> Hash["🔐 Hash final<br/>$2b$10$... (bcrypt)"]

    style Passwd fill:#e1f5fe
    style Salt fill:#fff3e0
    style KDF fill:#c8e6c9
    style Cost fill:#fce4ec
    style Hash fill:#e8f5e9
```

### ¿Qué es el "salt"?

Una cadena aleatoria única que se añade a cada contraseña antes de hashearla. Así, dos usuarios con la misma contraseña tienen **hashes completamente diferentes**.

```mermaid
graph LR
    subgraph SinSalt ["🚫 Sin salt"]
        A1["👤 Ana: 'password'"] --> H1["hash1: a1b2c3..."]
        B1["👤 Bob: 'password'"] --> H2["hash2: a1b2c3..."]
        Note1["⚠️ Mismo hash → si un<br/>empleado conoce el hash<br/>de Ana, sabe la de Bob"]
    end

    subgraph ConSalt ["✅ Con salt"]
        A2["👤 Ana: 'password'+salt1"] --> H3["hash3: x9y8z7..."]
        B2["👤 Bob: 'password'+salt2"] --> H4["hash4: m4n5b6..."]
        Note2["✅ Hashes diferentes<br/>aunque la contraseña<br/>sea la misma"]
    end

    style SinSalt fill:#ffcdd2
    style ConSalt fill:#c8e6c9
```

---

## bcrypt

**bcrypt** es el algoritmo más usado para almacenar contraseñas. Está diseñado específicamente para este propósito: es **lento, incluye salt automático** y su costo es ajustable.

```bash
# Ejemplo de hash bcrypt
$2b$10$A8f5B7C2dE9fG1hI3jK4lM5nO6pQ7rS8tU9vW0xY1zA2bC3dE4fG5hI6j
├─┤├─┤├────────────────────────────────────────────────────────────┤
│  │  └── Hash + Salt (60 caracteres)
│  └── Factor de costo (10 = 2^10 iteraciones)
└── Versión (2b)
```

### Factor de costo

El factor de costo determina cuántas iteraciones hace bcrypt. Cada incremento duplica el tiempo.

| Costo | Iteraciones | Tiempo aprox. |
| ----- | ----------- | -------------- |
| 10    | 1,024       | ~100ms         |
| 12    | 4,096       | ~400ms         |
| 14    | 16,384      | ~1.6s          |

```javascript
// Node.js con bcrypt
const bcrypt = require('bcrypt');

// Hashear (automáticamente genera salt)
const hash = await bcrypt.hash('MiPassword123', 12);

// Verificar
const coincide = await bcrypt.compare('MiPassword123', hash);
// → true / false
```

```python
# Python con bcrypt
import bcrypt

# Hashear
hash = bcrypt.hashpw(b'MiPassword123', bcrypt.gensalt(rounds=12))

# Verificar
coincide = bcrypt.checkpw(b'MiPassword123', hash)
# → True / False
```

> **bcrypt es el estándar de facto** para contraseñas. Si no sabes cuál usar, usa bcrypt.

---

## scrypt

**scrypt** es similar a bcrypt pero además de ser costoso en CPU, también requiere **mucha memoria RAM**, lo que hace extremadamente difícil el ataque con GPUs o ASICs.

```mermaid
flowchart TB
    subgraph ComparativaKDF ["⚖️ bcrypt vs scrypt vs argon2"]
        B["🔵 bcrypt<br/>CPU-bound<br/>Poca RAM<br/>Estándar actual"]
        S["🟢 scrypt<br/>CPU + RAM<br/>Dificulta GPUs/ASICs<br/>Mejor que bcrypt"]
        A["🔴 argon2<br/>CPU + RAM + paralelismo<br/>Ganador de competición PHC<br/>Lo más seguro hoy"]
    end

    style ComparativaKDF fill:#f5f5f5
    style B fill:#fff3e0
    style S fill:#c8e6c9
    style A fill:#e8eaf6
```

```bash
# Hash scrypt
$s0$e0801$W9j8f7d6g5h4j3k2l1m0n9b8v7c6x5z4a3s2d1f0g1h2j3k4l5m6n7b8v9c0x
```

```javascript
// Node.js con scrypt (nativo desde Node 10)
const { scryptSync, randomBytes, timingSafeEqual } = require('crypto');

const salt = randomBytes(16).toString('hex');
const hash = scryptSync('MiPassword123', salt, 64).toString('hex');

// Guardas: salt + ":" + hash
const almacenado = `${salt}:${hash}`;
```

---

## argon2

**argon2** es el ganador de la competición PHC (Password Hashing Competition) en 2015. Es el algoritmo más seguro y moderno para contraseñas.

```javascript
// Node.js con argon2
const argon2 = require('argon2');

const hash = await argon2.hash('MiPassword123', {
    type: argon2.argon2id,  // recomendado
    memoryCost: 19456,       // 19 MB
    timeCost: 2,
    parallelism: 1
});

const ok = await argon2.verify(hash, 'MiPassword123');
```

```python
# Python con argon2-cffi
from argon2 import PasswordHasher

ph = PasswordHasher()
hash = ph.hash("MiPassword123")
ok = ph.verify(hash, "MiPassword123")
```

---

## Comparativa: ¿cuál usar?

```mermaid
flowchart TD
    P{🔍 ¿Para qué necesitas hash?}

    P -->|"Contraseñas de usuarios"| Contrasenas["🔐 Almacenar contraseñas"]
    Contrasenas --> Elegir{"¿Qué priorizas?"}
    Elegir -->|"Estándar, más soporte"| BCRYPT["✅ bcrypt<br/>El más usado, soporte en todo"]
    Elegir -->|"Máxima seguridad"| ARGON2["✅ argon2<br/>Lo más seguro hoy"]
    Elegir -->|"Resistencia a GPU/ASIC"| SCRYPT["✅ scrypt<br/>Mucha RAM requerida"]

    P -->|"Integridad de archivos"| Integridad["📦 Checksums"]
    Integridad --> SHA256["✅ SHA-256"]
    Integridad --> SHA512["✅ SHA-512"]

    P -->|"Firmas / Certificados"| Firmas["🖊️ Firmas digitales"]
    Firmas --> SHA256_2["✅ SHA-256 (dentro de RSA/ECDSA)"]

    P -->|"NO uses para nada"| MD5["❌ MD5 - completamente roto"]
    P -->|"NO uses"| SHA1["❌ SHA-1 - colisiones demostradas"]

    style P fill:#e1f5fe
    style BCRYPT fill:#c8e6c9
    style ARGON2 fill:#c8e6c9
    style SCRYPT fill:#c8e6c9
    style SHA256 fill:#c8e6c9
    style SHA256_2 fill:#c8e6c9
    style MD5 fill:#ffcdd2
    style SHA1 fill:#ffcdd2
```

### Tabla resumen

| Algoritmo | ¿Para contraseñas? | Velocidad   | Seguridad      | Uso recomendado           |
| --------- | ------------------ | ----------- | -------------- | ------------------------- |
| **MD5**   | ❌ No              | ⚡ Muy rápida | ❌ Roto       | Solo checksums no críticos |
| **SHA-1** | ❌ No              | ⚡ Muy rápida | ❌ Roto       | Nada (migrar a SHA-2)     |
| **SHA-256** | ❌ No            | ⚡ Rápida    | ✅ Criptográfico | Firma, TLS, integridad  |
| **bcrypt**  | ✅ Sí           | 🐢 Lenta     | ✅ Seguro      | **Estándar para pw**      |
| **scrypt**  | ✅ Sí           | 🐢 Lenta + RAM | ✅ Más seguro | Resistencia a hardware    |
| **argon2**  | ✅ Sí           | 🐢 Lenta + RAM | ✅✅ Más seguro | Lo más moderno           |

---

## Recomendaciones finales

```mermaid
flowchart LR
    Pass["🔤 Contraseña del usuario"] --> Hash["🔐 Hash con bcrypt / argon2"]
    Hash --> Store["🗄️ Guardas en BD:<br/>$2b$12$... (solo el hash)"]
    Login["🔑 Usuario inicia sesión"] --> Fetch["🗄️ Traes hash de la BD"]
    Fetch --> Compare["🔄 bcrypt.compare(pass, hash)"]
    Compare -->|"✅ true"| Ok["🎉 Acceso concedido"]
    Compare -->|"❌ false"| Deny["🚫 Acceso denegado"]

    style Pass fill:#e1f5fe
    style Hash fill:#fff3e0
    style Store fill:#c8e6c9
    style Login fill:#fce4ec
    style Compare fill:#fff9c4
    style Ok fill:#c8e6c9
    style Deny fill:#ffcdd2
```

### Checklist de seguridad para contraseñas

- [ ] Usa **bcrypt** (costo ≥ 12) o **argon2**
- [ ] **Nunca** uses MD5, SHA-1, SHA-256 para contraseñas
- [ ] **Nunca** almacenes contraseñas en texto plano
- [ ] **Nunca** encriptes contraseñas (la encriptación se puede desencriptar)
- [ ] **Siempre** usa salt (bcrypt/argon2 lo generan automáticamente)
- [ ] **HTTPS** en toda comunicación
- [ ] **Rate limiting** en login para prevenir fuerza bruta

> **Siguiente paso:** Reemplaza cualquier hash inseguro en tus proyectos con bcrypt o argon2, y estudia el [OWASP Top 10](https://owasp.org/www-project-top-ten/) para conocer las vulnerabilidades web más críticas.
