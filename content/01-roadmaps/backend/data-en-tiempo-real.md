# Data en tiempo real

Las aplicaciones modernas necesitan **comunicación en tiempo real**: chats, notificaciones, dashboards en vivo, juegos multijugador. Existen varias técnicas para lograr que el servidor envíe datos al cliente **sin que el cliente los pida explícitamente**.

```mermaid
flowchart TB
    RealTime["⚡ Comunicación en Tiempo Real"]

    RealTime --> Polling["🔄 Polling<br/>Cliente pregunta cada N segundos"]
    RealTime --> SSE["📡 Server-Sent Events<br/>Servidor empuja datos<br/>(unidireccional)"]
    RealTime --> WebSocket["🔗 WebSockets<br/>Conexión bidireccional<br/>permanente"]

    Polling --> ShortPolling["⏱️ Short Polling<br/>Pregunta cada 1-5s"]
    Polling --> LongPolling["🕒 Long Polling<br/>Espera hasta que haya datos"]

    style RealTime fill:#e1f5fe
    style Polling fill:#fff3e0
    style SSE fill:#c8e6c9
    style WebSocket fill:#fce4ec
```

---

## Polling (Short y Long Polling)

El **polling** es la técnica más simple: el cliente pregunta al servidor repetidamente si hay datos nuevos.

### Short Polling

El cliente hace peticiones **cada N segundos** independientemente de si hay datos nuevos.

```mermaid
sequenceDiagram
    participant Client as 📱 Cliente
    participant Server as 🖥️ Servidor

    Client->>Server: GET /nuevos-mensajes?desde=id1
    Server-->>Client: [] (sin datos)

    Note over Client: Espera 3 segundos...

    Client->>Server: GET /nuevos-mensajes?desde=id1
    Server-->>Client: [] (sin datos)

    Note over Client: Espera 3 segundos...

    Client->>Server: GET /nuevos-mensajes?desde=id1
    Server-->>Client: [{ id: 2, texto: "Hola!" }]

    Note over Client: ✅ Datos recibidos
```

```javascript
// Short Polling en el cliente
async function checkMessages() {
    const res = await fetch('/api/messages?since=lastId');
    const messages = await res.json();

    if (messages.length > 0) {
        showMessages(messages);
    }
}

// Preguntar cada 3 segundos
setInterval(checkMessages, 3000);
```

| Ventajas                         | Desventajas                          |
| -------------------------------- | ------------------------------------ |
| ✅ Simple de implementar          | ❌ Muchas peticiones innecesarias    |
| ✅ Funciona en cualquier servidor | ❌ Latencia de hasta N segundos       |
| ✅ Sin conexiones persistentes    | ❌ Alto consumo de ancho de banda    |

### Long Polling

El cliente hace una petición y el servidor **mantiene la conexión abierta** hasta que haya datos (o hasta un timeout).

```mermaid
sequenceDiagram
    participant Client as 📱 Cliente
    participant Server as 🖥️ Servidor

    Client->>Server: POST /poll
    Note over Server: No hay datos... espera

    Server-->>Client: (conexión abierta, esperando)

    Note over Server: Llega un nuevo mensaje

    Server-->>Client: [{ id: 2, texto: "Hola!" }]
    Client->>Server: POST /poll (siguiente)

    Note over Client: Inmediatamente vuelve a preguntar

    Server-->>Client: (espera...)
```

```javascript
// Long Polling
async function longPoll() {
    try {
        const res = await fetch('/api/poll');
        const data = await res.json();
        processData(data);
    } catch (err) {
        // timeout o error, reconectar
        console.log('Reconectando...');
    } finally {
        longPoll(); // inmediatamente vuelve a preguntar
    }
}

longPoll();
```

| Ventajas                       | Desventajas                         |
| ------------------------------ | ----------------------------------- |
| ✅ Latencia menor que short polling | ❌ Mantiene conexiones abiertas   |
| ✅ Más eficiente que short polling | ❌ Timeout y reconexión compleja |
| ✅ Mensajería casi instantánea  | ❌ No escala tan bien como WebSocket |

---

## Server-Sent Events (SSE)

**SSE** permite que el servidor **empuje datos al cliente** a través de una conexión HTTP larga. Es **unidireccional** (servidor → cliente). Más simple que WebSocket pero sin comunicación bidireccional.

```mermaid
sequenceDiagram
    participant Client as 📱 Cliente
    participant Server as 🖥️ Servidor

    Client->>Server: GET /events (text/event-stream)
    Server-->>Client: Conexión establecida

    Note over Server: Llega un mensaje

    Server-->>Client: data: { "user": "Ana", "text": "Hola!" }

    Note over Server: Llega otro mensaje

    Server-->>Client: data: { "user": "Bob", "text": "Qué tal!" }

    Note over Client: El cliente recibe datos<br/>automáticamente
```

### Implementación

```javascript
// Servidor (Node.js + Express)
app.get('/events', (req, res) => {
    res.writeHead(200, {
        'Content-Type': 'text/event-stream',
        'Cache-Control': 'no-cache',
        'Connection': 'keep-alive'
    });

    // Enviar evento cada vez que hay datos
    const interval = setInterval(() => {
        const data = { time: new Date().toISOString() };
        res.write(`data: ${JSON.stringify(data)}\n\n`);
    }, 1000);

    req.on('close', () => clearInterval(interval));
});
```

```javascript
// Cliente (navegador)
const eventSource = new EventSource('/events');

eventSource.onmessage = (event) => {
    const data = JSON.parse(event.data);
    console.log('Nuevo dato:', data);
};

eventSource.onerror = () => {
    console.log('Error de conexión, reconectando...');
    // SSE reconecta automáticamente
};
```

| Ventajas                       | Desventajas                          |
| ------------------------------ | ------------------------------------ |
| ✅ API nativa del navegador (EventSource) | ❌ Solo unidireccional (server → cliente) |
| ✅ Reconexión automática        | ❌ Límite de conexiones por navegador |
| ✅ Simple, sobre HTTP           | ❌ No funciona si el cliente necesita enviar datos |
| ✅ Ideal para notificaciones, feeds | ❌ No soportado en HTTP/1.1 con muchos clientes |

---

## WebSockets

**WebSockets** proporcionan una **conexión bidireccional y permanente** entre cliente y servidor. Ambos pueden enviar mensajes en cualquier momento.

```mermaid
sequenceDiagram
    participant Client as 📱 Cliente
    participant Server as 🖥️ Servidor

    Client->>Server: HTTP Upgrade Request
    Server-->>Client: 101 Switching Protocols

    Note over Client,Server: 🔗 Conexión WebSocket establecida

    Client->>Server: Mensaje: "Hola servidor!"
    Server-->>Client: Mensaje: "Hola cliente! 👋"

    Server-->>Client: Notificación: "Nuevo usuario conectado"

    Client->>Server: Mensaje: "Adiós!"
    Note over Client,Server: Conexión cerrada

    Note over Client,Server: Comunicación bidireccional<br/>en tiempo real
```

### Implementación

```javascript
// Servidor (Node.js + ws)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
    console.log('Cliente conectado');

    // Recibir mensajes del cliente
    ws.on('message', (data) => {
        console.log('Recibido:', data.toString());

        // Responder solo a ese cliente
        ws.send(`Servidor recibió: ${data}`);

        // O broadcast a todos los clientes
        wss.clients.forEach(client => {
            if (client.readyState === WebSocket.OPEN) {
                client.send(`Broadcast: ${data}`);
            }
        });
    });

    // Enviar mensaje al conectarse
    ws.send('Bienvenido al servidor WebSocket!');
});
```

```javascript
// Cliente (navegador)
const ws = new WebSocket('ws://localhost:8080');

ws.onopen = () => {
    console.log('Conectado!');
    ws.send('Hola servidor!');
};

ws.onmessage = (event) => {
    console.log('Mensaje del servidor:', event.data);
};

ws.onclose = () => {
    console.log('Desconectado');
};

ws.onerror = (err) => {
    console.error('Error:', err);
};
```

### WebSocket en el navegador vs Socket.IO

```mermaid
flowchart TB
    subgraph WebSocketLibraries ["📚 Librerías WebSocket"]
        Native["🌐 WebSocket nativo<br/>(ws en servidor,<br/>new WebSocket() en cliente)"]
        SocketIO["🔌 Socket.IO<br/>(Node.js)<br/>✅ Auto-reconexión<br/>✅ Rooms, namespaces<br/>✅ Fallback a polling"]
        WS["📦 ws (Node.js)<br/>✅ Ligero<br/>✅ Bien para microservicios"]
    end

    style WebSocketLibraries fill:#f5f5f5
```

### Socket.IO (ejemplo rápido)

```javascript
// Servidor
const io = require('socket.io')(3000);

io.on('connection', (socket) => {
    socket.on('mensaje', (data) => {
        io.emit('mensaje', data); // broadcast
    });

    socket.join('sala-1'); // unirse a sala
    io.to('sala-1').emit('evento', { data: 'solo sala 1' });
});

// Cliente
const socket = io('http://localhost:3000');
socket.emit('mensaje', { texto: 'Hola!' });
socket.on('mensaje', (data) => console.log(data));
```

---

## Comparativa

```mermaid
flowchart TB
    Elegir{"🔍 ¿Qué necesitas?"}

    Elegir -->|"Simple, datos cada N segundos"| SP["🔄 Short Polling<br/>Fácil, pero ineficiente"]
    Elegir -->|"Casi tiempo real,<br/>sin conexión permanente"| LP["🕒 Long Polling<br/>Mejor, pero más complejo"]
    Elegir -->|"Notificaciones one-way<br/>del servidor al cliente"| SSE2["📡 SSE<br/>Simple y eficiente"]
    Elegir -->|"Chat, juegos,<br/>bidireccional en tiempo real"| WS2["🔗 WebSocket<br/>El estándar para tiempo real"]

    style Elegir fill:#e1f5fe
    style SP fill:#fff3e0
    style LP fill:#ffe0b2
    style SSE2 fill:#c8e6c9
    style WS2 fill:#fce4ec
```

### Tabla comparativa

| Característica         | Short Polling | Long Polling  | SSE            | WebSocket      |
| ---------------------- | ------------- | ------------- | -------------- | -------------- |
| **Dirección**          | Cliente → Servidor | Cliente → Servidor | Servidor → Cliente | Bidireccional |
| **Latencia**           | Segundos       | ~Instantáneo  | Instantáneo    | Instantáneo    |
| **Conexión persistente**| ❌            | ✅ (parcial)  | ✅             | ✅             |
| **Complejidad**        | Muy baja       | Media         | Baja           | Media-Alta     |
| **Navegador nativo**   | ✅ fetch       | ✅ fetch      | ✅ EventSource | ✅ WebSocket API |
| **Reconexión auto**    | ❌             | ❌            | ✅             | ❌ (Socket.IO sí) |
| **Escalabilidad**      | Alta           | Media         | Media          | Media (más conexiones) |
| **Ideal para**         | Datos que cambian lento | Notificaciones casi reales | Feeds, precios, logs | Chat, juegos, colaboración |

---

## Buenas prácticas

```mermaid
flowchart LR
    subgraph Best ["✅ Buenas prácticas"]
        B1["Reconexión automática<br/>con backoff exponencial"]
        B2["Heartbeat / Ping-Pong<br/>para detectar desconexión"]
        B3["Limitar conexiones<br/>por IP / usuario"]
        B4["Comprimir mensajes<br/>si son grandes"]
        B5["Usar WSS (WebSocket<br/>sobre TLS)"]
    end

    style Best fill:#c8e6c9
```

### Backoff exponencial para reconexión

```javascript
function connectWebSocket(retries = 0) {
    const ws = new WebSocket('ws://localhost:8080');

    ws.onclose = () => {
        const delay = Math.min(1000 * Math.pow(2, retries), 30000);
        console.log(`Reconectando en ${delay}ms...`);
        setTimeout(() => connectWebSocket(retries + 1), delay);
    };

    ws.onopen = () => {
        console.log('Conectado!');
        retries = 0; // resetear al reconectar
    };
}
```

---

## Resumen visual

```mermaid
graph TB
    RT["⚡ Comunicación en Tiempo Real"]

    RT --> Poll["🔄 Polling"]
    Poll --> Short["Short: Pregunta cada N segundos<br/>🐌 Latencia: N segundos"]
    Poll --> Long2["Long: Conexión abierta hasta datos<br/>⚡ Latencia: casi instantánea"]

    RT --> SSE3["📡 Server-Sent Events"]
    SSE3 --> Uni["Unidireccional (server → cliente)"]
    SSE3 --> Use["Notificaciones, feeds, precios"]

    RT --> WS3["🔗 WebSockets"]
    WS3 --> Bi["Bidireccional"]
    WS3 --> Use2["Chat, juegos, colaboración,<br/>dashboards en vivo"]

    RT --> Rule["🤔 ¿Cuál elegir?"]
    Rule --> Simple2["¿Solo mostrar datos periódicos? → Polling"]
    Rule --> Notify["¿Notificar eventos del servidor? → SSE"]
    Rule --> Chat2["¿Chat / interacción 2 vías? → WebSocket"]

    style RT fill:#e1f5fe
    style Poll fill:#fff3e0
    style SSE3 fill:#c8e6c9
    style WS3 fill:#fce4ec
    style Rule fill:#f5f5f5
```

> **Siguiente paso:** Implementa SSE para notificaciones en vivo o WebSockets para un chat simple. Prueba Socket.IO si tu app Node.js necesita reconexión automática y rooms.

## Relacionados:
- [[patrones-arquitectonicos]] #anterior 
- [[escalando-bases-de-datos]] #siguiente