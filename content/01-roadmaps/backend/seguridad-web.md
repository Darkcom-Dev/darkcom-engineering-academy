# Seguridad Web

La seguridad web abarca las prácticas, protocolos y herramientas para proteger aplicaciones web contra ataques, fugas de datos y accesos no autorizados.

```mermaid
flowchart TB
    SW["🛡️ Seguridad Web"] --> HTTPS["🔒 HTTPS / TLS<br/>Comunicación encriptada"]
    SW --> CORS["🌐 CORS<br/>Control de acceso cruzado"]
    SW --> CSP["📋 Content Security Policy<br/>Mitigar XSS"]
    SW --> OWASP["⚠️ OWASP Top 10<br/>Vulnerabilidades críticas"]
    SW --> Server["🖥️ Seguridad de servidores"]
    SW --> API["🔌 Mejores prácticas API"]

    style SW fill:#e1f5fe
    style HTTPS fill:#c8e6c9
    style CORS fill:#fff3e0
    style CSP fill:#e8eaf6
    style OWASP fill:#ffcdd2
    style Server fill:#f3e5f5
    style API fill:#e8f5e9
```

---

## HTTPS / SSL / TLS

**HTTPS** (HTTP over SSL/TLS) encripta toda la comunicación entre el navegador y el servidor. Sin HTTPS, cualquier dato viaja en texto plano y puede ser interceptado.

```mermaid
flowchart LR
    subgraph HTTP ["🚫 HTTP (sin encriptar)"]
        H1["Navegador"] -->|"🔓 Texto plano"| H2["Servidor"]
        Attacker1["👤 Atacante"] -.->|"👁️ Ve todo: passwords,<br/>tarjetas, cookies"| H1
    end

    subgraph HTTPS ["✅ HTTPS (encriptado)"]
        S1["Navegador"] -->|"🔒 Cifrado TLS"| S2["Servidor"]
        Attacker2["👤 Atacante"] -.->|"❌ Solo ve datos<br/>encriptados"| S1
    end

    style HTTP fill:#ffcdd2
    style HTTPS fill:#c8e6c9
    style Attacker1 fill:#ffcdd2
    style Attacker2 fill:#c8e6c9
```

> **Analogía:** HTTP es una postal (cualquiera la lee). HTTPS es una carta sellada (solo el destinatario la abre).

### TLS Handshake (apretón de manos)

```mermaid
sequenceDiagram
    participant Browser as 🌍 Navegador
    participant Server as 🖥️ Servidor

    Browser->>Server: ① ClientHello<br/>Versiones TLS, cifrados soportados
    Server-->>Browser: ② ServerHello<br/>Versión TLS, cifrado elegido
    Server-->>Browser: ③ Certificado SSL (público)
    Browser->>Browser: ④ Verifica certificado<br/>contra CA (autoridad certificadora)

    alt Certificado válido
        Browser->>Server: ⑤ Pre-master secret (encriptado con clave pública)
        Note over Browser,Server: Ambas partes generan la misma clave de sesión
        Browser-->>Server: ⑥ 🔒 Todo encriptado desde ahora
        Server-->>Browser: ⑦ 🔒
    else Certificado inválido
        Browser-->>Browser: ⚠️ Advertencia de seguridad
    end
```

### ¿Qué es un certificado SSL?

Es un archivo digital que **vincula una identidad (dominio) con una clave pública**. Es emitido por una **CA** (Certificate Authority) como Let's Encrypt, DigiCert, Cloudflare.

```
🔐 Certificado contiene:
├── Dominio: mipagina.com
├── Emitido por: Let's Encrypt
├── Válido: 01/01/2025 - 01/04/2025
├── Clave pública: [clave para encriptar]
└── Firma digital de la CA
```

### Cómo obtener HTTPS

```bash
# Let's Encrypt + Certbot (¡gratis!)
sudo certbot --nginx -d mipagina.com -d www.mipagina.com

# O con Caddy (HTTPS automático)
# Solo pones el dominio en el Caddyfile y ya.
```

---

## CORS

**CORS** (Cross-Origin Resource Sharing) es un mecanismo de seguridad del navegador que controla qué dominios pueden acceder a los recursos de tu servidor.

### El problema del mismo origen

```mermaid
graph LR
    MiApp["📱 miapp.com<br/>(frontend)"] --> API["🔌 api.miapp.com<br/>(backend)"]
    MiApp --> Otro["🌐 otro-sitio.com<br/>(intenta llamar a tu API)"]

    Otro -.->|"🚫 Bloqueado por CORS"| API
    MiApp -->|"✅ Permitido por CORS"| API

    style MiApp fill:#c8e6c9
    style API fill:#e1f5fe
    style Otro fill:#ffcdd2
```

### Cómo funciona CORS

El navegador envía una cabecera `Origin` y el servidor responde si acepta o no ese origen.

```http
# Petición desde otro-sitio.com
GET /api/usuarios HTTP/1.1
Origin: https://otro-sitio.com

# Respuesta del servidor
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://miapp.com
#                             ↑ solo miapp.com tiene acceso
# O también:
Access-Control-Allow-Origin: *   # cualquiera (público)
```

### Preflight (OPTIONS)

Para peticiones "complejas" (métodos distintos a GET/POST, cabeceras personalizadas), el navegador primero envía una petición **OPTIONS** de verificación.

```mermaid
sequenceDiagram
    participant Browser as 🌍 Navegador (miapp.com)
    participant API as 🔌 API (api.miapp.com)

    Browser->>API: ① OPTIONS /api/usuarios<br/>Origin: https://miapp.com<br/>Access-Control-Request-Method: DELETE
    API-->>Browser: ② 204 No Content<br/>Access-Control-Allow-Origin: https://miapp.com<br/>Access-Control-Allow-Methods: GET, POST, DELETE
    Browser->>API: ③ DELETE /api/usuarios/1<br/>Origin: https://miapp.com
    API-->>Browser: ④ 200 OK<br/>Access-Control-Allow-Origin: https://miapp.com
```

### Configuración CORS en backend

```javascript
// Node.js (Express)
app.use(cors({
    origin: 'https://miapp.com',
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    allowedHeaders: ['Content-Type', 'Authorization'],
    credentials: true
}));
```

```python
# FastAPI
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://miapp.com"],
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### Buenas prácticas CORS

- **No uses `*` en producción** — especifica orígenes exactos
- **Credenciales** — si usas cookies, necesitas `credentials: true` y origen específico (no `*`)
- **Solo expón lo necesario** — no permitas métodos que no uses

---

## CSP (Content Security Policy)

**CSP** es una cabecera HTTP que controla **qué fuentes** (scripts, estilos, imágenes, etc.) puede cargar el navegador. Es la defensa más efectiva contra **XSS** (Cross-Site Scripting).

```mermaid
flowchart TB
    subgraph SinCSP ["🚫 Sin CSP"]
        Atacante["👤 Atacante"] --> Inyecta["💉 Inyecta script malicioso<br/><script>enviarDatos()</script>"]
        Inyecta --> Ejecuta["⚡ El navegador lo ejecuta<br/>sin preguntar"]
        Ejecuta --> Robo["💀 Datos robados"]
    end

    subgraph ConCSP ["✅ Con CSP"]
        Atacante2["👤 Atacante"] --> Inyecta2["💉 Inyecta script"]
        Inyecta2 --> Bloquea["🚫 CSP bloquea la ejecución<br/>Script no está en<br/>lista blanca"]
        Bloquea --> Seguro["✅ Usuario seguro"]
    end

    style SinCSP fill:#ffcdd2
    style ConCSP fill:#c8e6c9
```

### Ejemplo de CSP

```http
Content-Security-Policy:
    default-src 'self';
    script-src 'self' https://cdn.jsdelivr.net;
    style-src 'self' https://fonts.googleapis.com;
    img-src 'self' https://images.example.com;
    font-src 'self' https://fonts.gstatic.com;
    connect-src 'self' https://api.miapp.com;
    frame-ancestors 'none';
```

| Directiva     | Controla                                    |
| ------------- | ------------------------------------------- |
| `default-src` | Fallback para todas las fuentes             |
| `script-src`  | Scripts JS permitidos                       |
| `style-src`   | Hojas de estilo permitidas                  |
| `img-src`     | Imágenes permitidas                         |
| `connect-src` | Conexiones fetch/XHR/WebSocket              |
| `font-src`    | Fuentes tipográficas                        |
| `frame-ancestors` | Quién puede incrustar en iframe (protege clickjacking) |

### Implementación

```javascript
// Node.js (helmet)
const helmet = require('helmet');
app.use(helmet.contentSecurityPolicy({
    directives: {
        defaultSrc: ["'self'"],
        scriptSrc: ["'self'", "https://cdn.jsdelivr.net"],
        // ...
    }
}));
```

---

## Riesgos OWASP

El **OWASP Top 10** es la lista de las vulnerabilidades web más críticas, actualizada por la comunidad de seguridad.

```mermaid
flowchart TB
    OWASP["⚠️ OWASP Top 10"] --> A1["① Broken Access Control<br/>Usuarios acceden a recursos<br/>que no deberían"]
    OWASP --> A2["② Cryptographic Failures<br/>Datos sensibles no encriptados"]
    OWASP --> A3["③ Injection<br/>SQLi (SQL Injection)"]
    OWASP --> A4["④ Insecure Design<br/>Fallas en el diseño de seguridad"]
    OWASP --> A5["⑤ Security Misconfiguration<br/>Configuraciones por defecto"]
    OWASP --> A6["⑥ Vulnerable Components<br/>Librerías desactualizadas"]
    OWASP --> A7["⑦ Auth Failures<br/>Fallas de autenticación"]
    OWASP --> A8["⑧ Data Integrity Failures<br/>Firmas no verificadas"]
    OWASP --> A9["⑨ Logging & Monitoring<br/>Falta de registro de ataques"]
    OWASP --> A10["⑩ SSRF<br/>Server-Side Request Forgery"]

    style OWASP fill:#ffcdd2
```

### Los más críticos para empezar

```mermaid
graph LR
    subgraph SQLi["💉 SQL Injection"]
        SQLi1["Input: '; DROP TABLE usuarios; --"]
        SQLi2["⚠️ Pueden eliminar tu BD"]
    end

    subgraph XSS["💀 Cross-Site Scripting"]
        XSS1["Input: &lt;script&gt;stealCookies()&lt;/script&gt;"]
        XSS2["⚠️ Roban sesiones de usuarios"]
    end

    subgraph CSRF["🔄 Cross-Site Request Forgery"]
        CSRF1["Un sitio malicioso hace<br/>POST a tu API sin que el<br/>usuario lo sepa"]
        CSRF2["⚠️ Cambian email, password, etc."]
    end

    style SQLi fill:#ffcdd2
    style XSS fill:#ffcdd2
    style CSRF fill:#ffcdd2
```

### Cómo mitigarlos

| Vulnerabilidad       | Mitigación                                       |
| -------------------- | ------------------------------------------------ |
| **SQL Injection**    | Usar **query parametrizado** (ORM/ prepared statements), nunca concatenar strings |
| **XSS**              | CSP, escapar output, validar input, cookies HttpOnly |
| **CSRF**             | Tokens CSRF, SameSite=Strict/Lax, verificar Origin/Referer |
| **Broken Access Control** | Validar permisos en cada endpoint, no confiar en parámetros del cliente |
| **Security Misconfiguration** | Deshabilitar servicios innecesarios, cabeceras de seguridad |

#### Ejemplo: SQL Injection vs Query parametrizado

```javascript
// ❌ VULNERABLE (no hagas esto)
const query = `SELECT * FROM usuarios WHERE email = '${email}'`;
// Input: email = "'; DROP TABLE usuarios; --"
// Resultado: ¡BD destruida!

// ✅ SEGURO (query parametrizado)
const query = 'SELECT * FROM usuarios WHERE email = ?';
db.query(query, [email]);
```

---

## Seguridad de servidores

Proteger el servidor donde corre tu aplicación es tan importante como escribir código seguro.

```mermaid
flowchart TB
    ServerSec["🖥️ Seguridad de servidores"]
    ServerSec --> Updates["🔄 Mantener SO actualizado<br/>apt update && apt upgrade"]
    ServerSec --> SSH["🔑 SSH con clave pública<br/>Deshabilitar login por password"]
    ServerSec --> Firewall["🧱 Firewall (ufw/iptables)<br/>Solo puertos necesarios: 80, 443, 22"]
    ServerSec --> Fail2Ban["🚫 fail2ban<br/>Bloquear IPs tras intentos fallidos"]
    ServerSec --> Users["👤 Usuarios<br/>No usar root, crear usuarios con sudo"]
    ServerSec --> Ports["🔌 Puertos<br/>Cambiar SSH de 22, cerrar puertos no usados"]

    style ServerSec fill:#f3e5f5
```

### Comandos esenciales de hardening

```bash
# Firewall (ufw)
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp       # SSH
sudo ufw allow 80/tcp       # HTTP
sudo ufw allow 443/tcp      # HTTPS
sudo ufw enable

# fail2ban
sudo apt install fail2ban
sudo systemctl enable fail2ban

# SSH hardening
# /etc/ssh/sshd_config:
#   PermitRootLogin no
#   PasswordAuthentication no
#   PubkeyAuthentication yes
sudo systemctl restart sshd
```

### Cabeceras de seguridad HTTP

El servidor debe enviar estas cabeceras en todas las respuestas:

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'
Referrer-Policy: no-referrer
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

```javascript
// Node.js con helmet (incluye todo)
const helmet = require('helmet');
app.use(helmet());
```

```python
# Python con Django
SECURE_HSTS_SECONDS = 31536000
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_BROWSER_XSS_FILTER = True
```

---

## Mejores prácticas de seguridad para APIs

```mermaid
flowchart LR
    subgraph APISecurity ["🔌 Seguridad en APIs"]
        Auth["🔐 Autenticación y<br/>Autorización"]
        Rate["⏱️ Rate Limiting<br/>Protege de DDoS/abuso"]
        Validate["✅ Validación y<br/>sanitización de input"]
        Log["📝 Logging y<br/>monitoreo"]
        Headers2["📋 Cabeceras de<br/>seguridad"]
    end

    style APISecurity fill:#e8f5e9
```

### Checklist de seguridad para APIs

- [ ] **HTTPS obligatorio** — redirige todo HTTP a HTTPS
- [ ] **Autenticación** — JWT, OAuth 2.0, o similar. No APIs abiertas sin auth
- [ ] **Rate limiting** — límite de peticiones por IP/usuario por minuto
- [ ] **Validar todo input** — nunca confíes en datos del cliente
- [ ] **No exponer información interna** — mensajes de error genéricos, sin stack traces
- [ ] **CORS restrictivo** — solo orígenes necesarios
- [ ] **Logging** — registra intentos de acceso, errores, cambios importantes
- [ ] **IDOR prevention** — verifica que el usuario tenga permiso para el recurso que pide
- [ ] **API keys** — rotación periódica, no hardcodearlas

### Rate limiting example

```javascript
// Express + express-rate-limit
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
    windowMs: 15 * 60 * 1000,  // 15 minutos
    max: 100,                    // 100 peticiones por ventana
    message: 'Demasiadas peticiones, intenta más tarde'
});

app.use('/api/', limiter);
```

### Manejo seguro de errores

```javascript
// ❌ MAL: expone información interna
{ "error": "SQLSTATE[23000]: ... duplicate entry 'admin' for key 'username'" }

// ✅ BIEN: mensaje genérico
{ "error": "El nombre de usuario ya está en uso" }
```

---

## Resumen visual

```mermaid
graph TB
    SW["🛡️ Seguridad Web"] --> Red["🌐 En tránsito"] & App["📱 En la app"] & Server["🖥️ En el servidor"]

    Red --> HTTPS2["🔒 HTTPS / TLS<br/>Todo encriptado"]
    Red --> HSTS["📋 HSTS<br/>Forzar HTTPS siempre"]

    App --> CORS2["🌐 CORS<br/>Controlar orígenes"]
    App --> CSP2["📋 CSP<br/>Mitigar XSS"]
    App --> Auth2["🔐 Autenticación<br/>JWT, OAuth"]
    App --> Validate2["✅ Validación<br/>SQLi, XSS, input"]
    App --> Rate2["⏱️ Rate Limiting"]

    Server --> Updates2["🔄 Actualizaciones"]
    Server --> Firewall2["🧱 Firewall"]
    Server --> SSH2["🔑 SSH con clave"]
    Server --> Headers3["📋 Cabeceras HTTP<br/>helmet / secure"]

    style SW fill:#e1f5fe
    style Red fill:#c8e6c9
    style App fill:#fff3e0
    style Server fill:#f3e5f5
```

### Principios básicos de seguridad

| Principio                 | Significado                                      |
| ------------------------- | ------------------------------------------------ |
| **Defense in depth**      | Múltiples capas de seguridad (ninguna es suficiente sola) |
| **Least privilege**       | Mínimos permisos necesarios para cada usuario/servicio |
| **Never trust user input**| Todo input del cliente es potencialmente malicioso |
| **Security by design**    | La seguridad se integra desde el diseño, no se añade después |
| **Keep it simple**        | Complejidad innecesaria = más superficie de ataque |

> **Siguiente paso:** Implementa helmet en tu backend, configura CORS correctamente, añade rate limiting, y estudia el [OWASP Top 10](https://owasp.org/www-project-top-ten/) en detalle.
