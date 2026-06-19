# Introducción

El **backend** es la parte de una aplicación que no vemos: el motor que procesa datos, ejecuta lógica de negocio y se comunica con bases de datos y otros servicios. Mientras el frontend es lo que el usuario ve y toca, el backend es lo que hace que todo funcione detrás de escena.

```mermaid
graph LR
    Usuario["👤 Usuario"] --> Frontend["🖥️ Frontend<br/>(HTML, CSS, JS)"]
    Frontend --> Backend["⚙️ Backend<br/>(Lógica, APIs)"]
    Backend --> DB["🗄️ Base de Datos"]
    Backend --> ServiciosExternos["🔌 Servicios<br/>Externos"]
    DB --> Backend
    ServiciosExternos --> Backend
    Backend --> Frontend
    Frontend --> Usuario

    style Usuario fill:#e1f5fe
    style Frontend fill:#fff3e0
    style Backend fill:#e8f5e9
    style DB fill:#fce4ec
    style ServiciosExternos fill:#f3e5f5
```

---

## ¿Cómo funciona Internet?

Internet es una **red global de computadoras interconectadas** que se comunican mediante protocolos estandarizados. Cuando accedes a un sitio web, tu dispositivo envía y recibe datos a través de múltiples nodos hasta llegar al servidor destino.

```mermaid
flowchart TB
    subgraph TuRed ["🌐 Tu Dispositivo"]
        PC["💻 Computadora"]
        Cel["📱 Celular"]
    end

    subgraph RedLocal ["🏠 Red Local"]
        Router["📡 Router/Modem"]
    end

    subgraph Internet ["☁️ Internet"]
        ISP["🏢 ISP<br/>(Proveedor)"]
        Nodos["🔀 Routers<br/>Intermedios"]
    end

    subgraph Servidor ["🎯 Servidor Destino"]
        Server["🖥️ Servidor Web"]
    end

    PC --> Router
    Cel --> Router
    Router --> ISP
    ISP --> Nodos
    Nodos --> Server

    style PC fill:#bbdefb
    style Cel fill:#bbdefb
    style Router fill:#ffe0b2
    style ISP fill:#c8e6c9
    style Nodos fill:#c8e6c9
    style Server fill:#ffcdd2
```

**El viaje de un paquete de datos:**

1. Tu computadora divide la información en **paquetes**
2. Cada paquete viaja por diferentes rutas (como si fueran cartas enviadas por caminos distintos)
3. Los **routers** intermedios reenvían los paquetes hacia su destino
4. El servidor destino reensambla los paquetes en el orden correcto

---

## ¿Qué es HTTP?

**HTTP** (HyperText Transfer Protocol) es el protocolo que permite la comunicación entre navegadores y servidores web. Es el "idioma" que hablan cliente y servidor para intercambiar información.

```mermaid
sequenceDiagram
    participant C as 🌍 Navegador (Cliente)
    participant S as 🖥️ Servidor Web

    Note over C,S: Protocolo HTTP/1.1 o HTTP/2

    C->>S: ① GET /index.html HTTP/1.1
    Note right of C: Headers: Host, User-Agent,<br/>Accept, Cookie...

    S-->>C: ② Respuesta HTTP
    Note left of S: Status: 200 OK<br/>Content-Type: text/html<br/>Content-Length: 4521

    S-->>C: ③ &lt;html&gt;&lt;body&gt;...&lt;/body&gt;&lt;/html&gt;
    Note right of C: El navegador renderiza<br/>el HTML recibido

    C->>S: ④ GET /styles.css HTTP/1.1
    S-->>C: ⑤ 200 OK + contenido CSS

    C->>S: ⑥ GET /app.js HTTP/1.1
    S-->>C: ⑦ 200 OK + contenido JS
```

### Métodos HTTP más comunes

| Método   | Acción       | Ejemplo                    |
| -------- | ------------ | -------------------------- |
| `GET`    | Obtener datos | Ver un artículo            |
| `POST`   | Crear recursos | Registrar un usuario       |
| `PUT`    | Actualizar todo | Editar perfil completo   |
| `PATCH`  | Actualizar parcial | Cambiar solo el email |
| `DELETE` | Eliminar      | Borrar una cuenta          |

### Códigos de estado HTTP

```mermaid
flowchart LR
    subgraph Informativas ["1xx Informativas"]
        A["100 Continue"]:::info
    end
    subgraph Exito ["2xx Éxito"]
        B["200 OK"]:::success
        C["201 Created"]:::success
    end
    subgraph Redireccion ["3xx Redirección"]
        D["301 Moved<br/>Permanently"]:::redir
        E["304 Not<br/>Modified"]:::redir
    end
    subgraph ErrorCliente ["4xx Error Cliente"]
        F["400 Bad<br/>Request"]:::error
        G["401 Unauthorized"]:::error
        H["403 Forbidden"]:::error
        I["404 Not Found"]:::error
    end
    subgraph ErrorServer ["5xx Error Servidor"]
        J["500 Internal<br/>Server Error"]:::serverr
        K["502 Bad<br/>Gateway"]:::serverr
        L["503 Service<br/>Unavailable"]:::serverr
    end

    classDef info fill:#e3f2fd,stroke:#1565c0
    classDef success fill:#e8f5e9,stroke:#2e7d32
    classDef redir fill:#fff8e1,stroke:#f57f17
    classDef error fill:#ffebee,stroke:#c62828
    classDef serverr fill:#fce4ec,stroke:#880e4f
```

---

## ¿Qué es el Dominio de Nombres?

El **DNS** (Domain Name System) es como la **agenda telefónica de Internet**. Los humanos recordamos nombres (`google.com`), pero las computadoras se comunican mediante direcciones IP numéricas (`142.250.184.14`). El DNS traduce un nombre en su IP correspondiente.

```mermaid
graph LR
    Humano["🧑 Nombre: google.com"] --> DNS["📖 DNS<br/>(Traductor)"]
    DNS --> IP["🔢 IP: 142.250.184.14"]

    subgraph Ejemplos [" "]
        A["facebook.com<br/>👉 157.240.1.35"]
        B["youtube.com<br/>👉 142.250.184.78"]
        C["github.com<br/>👉 140.82.121.3"]
    end

    style Humano fill:#e1f5fe
    style DNS fill:#fff9c4
    style IP fill:#f8bbd0
```

**Jerarquía de dominios:**

```mermaid
flowchart TD
A[".Raiz ← Servidores raíz (13 clusteres globales)"]
B[".com ← TLD (Top Level Domain)"]
C["google ← Dominio de segundo nivel"]
D["mail. ← Subdominio (opcional)"]
A --> B --> C --> D
```

> mail.google.com → leer de derecha a izquierda: `.` → `.com` → `google` → `mail.`


---

## ¿Qué es hosting?

El **hosting** (alojamiento web) es el servicio que proporciona **espacio en un servidor** para almacenar los archivos de tu sitio web y hacerlos accesibles desde Internet 24/7.

```mermaid
flowchart TB
    subgraph Tipos["🗂️ Tipos de Hosting"]
        Compartido["📦 Compartido"]
        VPS["🧊 VPS"]
        Dedicado["🖥️ Dedicado"]
        Cloud["☁️ Cloud"]
    end

    Hosting["🔧 Servicio de Hosting"]

    subgraph Componentes["Incluye"]
        Almacenamiento["💾 Almacenamiento"]
        RAM["🧠 RAM"]
        CPU["⚡ CPU"]
        AnchoBanda["📶 Ancho de Banda"]
        SSL["🔒 SSL"]
    end

    Compartido --> Hosting
    VPS --> Hosting
    Dedicado --> Hosting
    Cloud --> Hosting

    Hosting --> Almacenamiento
    Hosting --> RAM
    Hosting --> CPU
    Hosting --> AnchoBanda
    Hosting --> SSL
```

| Tipo       | Ideal para...              | Ejemplo                    |
| ---------- | -------------------------- | -------------------------- |
| Compartido | Blogs, sitios pequeños     | Bluehost, HostGator        |
| VPS        | Sitios en crecimiento      | DigitalOcean, Linode       |
| Dedicado   | Grandes aplicaciones       | OVH, Hetzner               |
| Cloud      | Aplicaciones variables     | AWS, Google Cloud, Azure   |

---

## ¿Cómo funciona el DNS?

Cuando escribes una URL en el navegador, ocurre toda una coreografía para resolver el nombre a una dirección IP.

```mermaid
sequenceDiagram
    participant User as 👤 Tú
    participant Browser as 🌍 Navegador
    participant Cache as 💾 Caché Local
    participant ISP as 🏢 DNS del ISP
    participant Root as 🌐 Servidor Raíz
    participant TLD as 📁 Servidor .com
    participant Authoritative as 🎯 Servidor Autoritativo

    User->>Browser: Escribe "google.com"

    Browser->>Cache: ¿Tienes la IP de google.com?

    alt Cache Hit
        Cache-->>Browser: IP: 142.250.184.14 ✅
    else Cache Miss
        Cache-->>Browser: No encontrado ❌
        Browser->>ISP: Resuelve google.com
        ISP->>Root: ¿Quién tiene .com?
        Root-->>ISP: Pregunta a los servidores .com

        ISP->>TLD: ¿Quién tiene google.com?
        TLD-->>ISP: Pregunta a ns1.google.com

        ISP->>Authoritative: Dame la IP de google.com
        Authoritative-->>ISP: IP: 142.250.184.14 ✅
        ISP-->>Browser: IP: 142.250.184.14
        Browser->>Cache: Guarda IP (TTL: 300s)
    end

    Browser->>Server: GET / HTTP/1.1 → 142.250.184.14
```

**Pasos resumidos:**

1. **Caché del navegador** → Si ya visitaste el sitio, usa la IP guardada
2. **Caché del SO** → Revisa la caché del sistema operativo
3. **DNS del ISP** → Pregunta al servidor DNS de tu proveedor
4. **Servidores raíz** → Indican dónde está el TLD (`.com`, `.org`, etc.)
5. **Servidor TLD** → Indica el servidor autoritativo del dominio
6. **Servidor autoritativo** → Devuelve la IP final

---

## ¿Cómo funcionan los navegadores?

El navegador es el **intérprete y visualizador** de la web. Su trabajo es tomar código (HTML, CSS, JS) y convertirlo en una página interactiva.

```mermaid
flowchart TB
    subgraph Entrada ["📥 Entrada"]
        URL["URL en la barra<br/>de direcciones"]
    end

    subgraph Procesamiento ["⚙️ Motor del Navegador"]
        DNS["① Resolución DNS<br/>(obtener IP)"]
        HTTP["② Solicitud HTTP<br/>(pedir recursos)"]
        Parse["③ Parseo<br/>(interpretar código)"]
    end

    subgraph Renderizado ["🎨 Renderizado"]
        DOM["🌳 DOM Tree<br/>(Estructura HTML)"]
        CSSOM["🎭 CSSOM<br/>(Estilos)"]
        RenderTree["🌲 Render Tree<br/>(Combinación)"]
        Paint["🖌️ Pintado<br/>(Píxeles en pantalla)"]
    end

    subgraph Interaccion ["🔄 Interacción"]
        JS["🧠 JavaScript<br/>Eventos, Animaciones<br/>Fetch, DOM API"]
    end

    URL --> DNS --> HTTP --> Parse
    Parse --> DOM
    Parse --> CSSOM
    DOM --> RenderTree
    CSSOM --> RenderTree
    RenderTree --> Paint
    Paint --> JS
    JS -.-> |"Modifica"| DOM
    JS -.-> |"Modifica"| CSSOM

    style Entrada fill:#e1f5fe
    style Procesamiento fill:#fff3e0
    style Renderizado fill:#e8f5e9
    style Interaccion fill:#fce4ec
```

### Componentes clave de un navegador

```mermaid
graph TB
    N["🌍 Navegador"] --> UI["🖥️ Interfaz de Usuario<br/>Barra de direcciones, botones"]
    N --> Engine["⚙️ Browser Engine<br/>Orquesta entre UI y Render"]
    N --> RenderEng["🎨 Render Engine<br/>Blink (Chrome), WebKit (Safari),<br/>Gecko (Firefox)"]
    N --> Network["🌐 Networking<br/>HTTP, FTP, DNS"]
    N --> JSInterp["🧠 JavaScript Interpreter<br/>V8 (Chrome), SpiderMonkey (FF),<br/>JavaScriptCore (Safari)"]
    N --> Storage["💾 Data Storage<br/>Cookies, LocalStorage,<br/>IndexedDB, Cache"]

    style N fill:#f3e5f5
    style UI fill:#bbdefb
    style Engine fill:#ffe0b2
    style RenderEng fill:#c8e6c9
    style Network fill:#fff9c4
    style JSInterp fill:#ffcdd2
    style Storage fill:#e1bdbd
```

### Flujo completo de una petición web

```mermaid
flowchart LR
    A["① Ingresas URL<br/>google.com"] --> B["② DNS Resuelve<br/>→ 142.250.184.14"]
    B --> C["③ Conexión TCP<br/>(apertura socket)"]
    C --> D["④ Handshake TLS<br/>(si es HTTPS)"]
    D --> E["⑤ Solicitud HTTP<br/>GET /index.html"]
    E --> F["⑥ Servidor procesa<br/>(backend)"]
    F --> G["⑦ Respuesta HTTP<br/>200 OK + HTML"]
    G --> H["⑧ Navegador<br/>parsea HTML"]
    H --> I["⑨ Solicita recursos<br/>CSS, JS, imágenes"]
    I --> J["⑩ Renderiza y<br/>ejecuta JS"]
    J --> K["🎉 ¡Página lista!"]

    style A fill:#e3f2fd
    style B fill:#e8f5e9
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f3e5f5
    style F fill:#e1f5fe
    style G fill:#e8f5e9
    style H fill:#fff3e0
    style I fill:#fce4ec
    style J fill:#f3e5f5
    style K fill:#c8e6c9
```

---

## Resumen visual: el viaje completo

```mermaid
flowchart TB
    U["👤 Usuario"] --> |"Escribe<br/>google.com"| B["🌍 Navegador"]
    B --> |"¿Cuál es la IP?"| DNS["📖 DNS"]
    DNS --> |"142.250.184.14"| B
    B --> |"GET / HTTP/1.1"| S["🖥️ Servidor<br/>(Hosting)"]
    S --> |"200 OK + HTML"| B
    B --> |"Renderiza"| U

    subgraph Internals ["🔍 Internamente el navegador"]
        B --> H["Parse HTML"]
        H --> H2["Solicita CSS, JS,<br/>imágenes"]
        H2 --> R["Render Tree"]
        R --> P["Paint 🎨"]
    end

    style U fill:#e1f5fe
    style B fill:#fff3e0
    style DNS fill:#e8f5e9
    style S fill:#fce4ec
    style Internals fill:#f5f5f5
```

### Conceptos clave para recordar

| Concepto       | Analogía                        |
| -------------- | ------------------------------- |
| **Internet**   | La red de carreteras mundial    |
| **HTTP**       | El idioma que hablan los barcos (datos) al viajar |
| **DNS**        | La guía telefónica que traduce nombres a números |
| **Hosting**    | El terreno donde construyes tu casa (sitio web) |
| **Navegador**  | El auto que te lleva a cualquier dirección |
| **Servidor**   | La casa que guarda y sirve la información |

---

> **Siguiente paso:** Aprende sobre protocolos más a fondo y cómo crear tu primer servidor HTTP con Node.js o Python.

## Relacionados:
- [[bases-del-frontend]]