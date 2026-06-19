# Broker de Mensajes

Un **broker de mensajes** (message broker) es un intermediario que permite que **servicios se comuniquen de forma asíncrona** enviando y recibiendo mensajes. Es el corazón de la arquitectura de microservicios.

```mermaid
flowchart LR
    S1["📱 Servicio A<br/>(pedidos)"] -->|"📤 Publica mensaje:<br/>'nuevo_pedido'"| Broker["🔌 Broker<br/>de Mensajes"]
    Broker -->|"📥 Consume"| S2["📱 Servicio B<br/>(pagos)"]
    Broker -->|"📥 Consume"| S3["📱 Servicio C<br/>(notificaciones)"]
    Broker -->|"📥 Consume"| S4["📱 Servicio D<br/>(inventario)"]

    style S1 fill:#e1f5fe
    style Broker fill:#ffcc80
    style S2 fill:#c8e6c9
    style S3 fill:#fff3e0
    style S4 fill:#fce4ec
```

### ¿Qué problema resuelve?

```mermaid
flowchart TB
    subgraph SinBroker ["🚫 Sin broker (síncrono)"]
        A1["Pedidos"] -->|"HTTP: cobrar()"| A2["Pagos"]
        A1 -->|"HTTP: notificar()"| A3["Notificaciones"]
        A1 -->|"HTTP: actualizar()"| A4["Inventario"]
        Note1["⚠️ Si Pagos falla, todo falla<br/>⚠️ Tiempo de respuesta = suma de todos<br/>⚠️ Acoplamiento total"]
    end

    subgraph ConBroker ["✅ Con broker (asíncrono)"]
        B1["Pedidos"] -->|"📤 Publica: pedido_creado"| B5["🔌 Broker"]
        B5 -->|"📥 Consume"| B2["Pagos"]
        B5 -->|"📥 Consume"| B3["Notificaciones"]
        B5 -->|"📥 Consume"| B4["Inventario"]
        Note2["✅ Si Pagos falla, el mensaje espera<br/>✅ Pedidos responde al instante<br/>✅ Desacoplamiento total"]
    end

    style SinBroker fill:#ffcdd2
    style ConBroker fill:#c8e6c9
```

### Conceptos clave

| Concepto       | Definición                                    |
| -------------- | --------------------------------------------- |
| **Mensaje**    | Unidad de datos que viaja por el broker       |
| **Productor**  | Quien envía mensajes                          |
| **Consumidor** | Quien recibe y procesa mensajes               |
| **Cola**       | Buffer donde los mensajes esperan ser procesados |
| **Exchange**   | Enruta mensajes a una o varias colas          |
| **Tópico**     | Categoría de mensajes (pub/sub)               |
| **ACK**        | Confirmación de que un mensaje fue procesado  |
| **DLQ**        | Dead Letter Queue: mensajes que fallaron       |

### Patrones de mensajería

```mermaid
flowchart TB
    Patrones["📐 Patrones"] --> Cola["📋 Cola simple<br/>1 productor → 1 consumidor<br/>(trabajo en cola)"]
    Patrones --> PubSub["📢 Pub/Sub<br/>1 productor → N consumidores<br/>(eventos)"]
    Patrones --> Routing["🔀 Routing<br/>Mensajes dirigidos por clave<br/>(errores → cola errores)"]
    Patrones --> RPC["📞 RPC<br/>Request/Reply<br/>(espera respuesta)"]

    style Patrones fill:#f5f5f5
```

---

## RabbitMQ

**RabbitMQ** es un broker de mensajes maduro, fácil de usar y basado en el protocolo **AMQP** (Advanced Message Queuing Protocol). Ideal para la mayoría de los casos de uso.

```mermaid
flowchart TB
    subgraph RabbitMQArch ["🐇 Arquitectura RabbitMQ"]
        Producer["📤 Productor"] --> Exchange["🔀 Exchange"]
        Exchange --> Queue1["📋 Cola: pedidos"] --> Consumer1["📥 Consumidor 1"]
        Exchange --> Queue2["📋 Cola: notificaciones"] --> Consumer2["📥 Consumidor 2"]
        Exchange --> Queue3["📋 Cola: errores"] --> Consumer3["📥 Consumidor 3"]
    end

    style RabbitMQArch fill:#fff3e0
    style Producer fill:#e1f5fe
    style Exchange fill:#ffcc80
```

### Tipos de Exchange

```mermaid
graph TB
    Exchange2["🔀 Tipos de Exchange"] --> Direct["Direct<br/>Ruteo exacto: 'error' → cola errores"]
    Exchange2 --> Topic["Topic<br/>Patrón: 'pedido.*' → cola pedidos"]
    Exchange2 --> Fanout["Fanout<br/>Broadcast a TODAS las colas"]
    Exchange2 --> Headers["Headers<br/>Ruteo por cabeceras (no routing key)"]

    style Exchange2 fill:#e1f5fe
    style Direct fill:#c8e6c9
    style Topic fill:#fff3e0
    style Fanout fill:#fce4ec
    style Headers fill:#e8eaf6
```

### Ejemplo Node.js

```javascript
const amqp = require('amqplib');

// PRODUCTOR
async function enviarPedido(pedido) {
    const conn = await amqp.connect('amqp://localhost');
    const channel = await conn.createChannel();
    const queue = 'pedidos';

    await channel.assertQueue(queue, { durable: true });
    channel.sendToQueue(queue, Buffer.from(JSON.stringify(pedido)), {
        persistent: true  // mensaje sobrevive a reinicios
    });

    console.log('Pedido enviado:', pedido.id);
}

// CONSUMIDOR
async function procesarPedidos() {
    const conn = await amqp.connect('amqp://localhost');
    const channel = await conn.createChannel();
    const queue = 'pedidos';

    await channel.assertQueue(queue, { durable: true });
    channel.consume(queue, (msg) => {
        const pedido = JSON.parse(msg.content.toString());
        console.log('Procesando pedido:', pedido.id);
        // ... lógica de negocio

        channel.ack(msg);  // confirmar procesado
    });
}
```

### Características de RabbitMQ

| Característica       | RabbitMQ                              |
| -------------------- | ------------------------------------- |
| **Protocolo**        | AMQP 0-9-1 (estándar)                 |
| **Persistencia**     | Sí (mensajes en disco)                |
| **Routing**          | Exchange + routing keys (flexible)    |
| **Lenguajes**        | Clientes para todos los lenguajes     |
| **Administración**   | UI web en puerto 15672                |
| **Ideal para**       | Colas de trabajo, comunicación entre servicios |

---

## Kafka

**Apache Kafka** es una plataforma de streaming distribuida diseñada para **alto rendimiento y gran volumen de datos**. No es solo un broker: es un **sistema de logs distribuido**.

```mermaid
flowchart TB
    subgraph KafkaArch ["⚡ Arquitectura Kafka"]
        P1["📤 Productor 1"]
        P2["📤 Productor 2"]
        P3["📤 Productor N"]

        P1 --> Topic["📂 Tópico particionado"]
        P2 --> Topic
        P3 --> Topic

        Topic --> Partition1["📦 Partición 0"]
        Topic --> Partition2["📦 Partición 1"]
        Topic --> Partition3["📦 Partición 2"]

        Partition1 --> C1["📥 Consumidor 1"]
        Partition2 --> C2["📥 Consumidor 2"]
        Partition3 --> C3["📥 Consumidor 3"]

        Group["👥 Grupo de consumidores"]
        C1 --> Group
        C2 --> Group
        C3 --> Group
    end

    style KafkaArch fill:#fce4ec
```

### Conceptos clave de Kafka

| Concepto        | Explicación                                      |
| --------------- | ------------------------------------------------ |
| **Tópico**      | Categoría de mensajes (ej: pedidos, eventos)     |
| **Partición**   | Subdivisión de un tópico (ordenada, inmutable)   |
| **Offset**      | Posición de un mensaje dentro de una partición   |
| **Consumer Group** | Grupo de consumidores que reparten particiones |
| **Retención**   | Los mensajes no se eliminan al consumirse (por tiempo/tamaño) |
| **Broker**      | Servidor Kafka                                   |

### La clave de Kafka: el log

```mermaid
flowchart LR
    subgraph Log ["📋 Log de Kafka (inmutable, ordenado)"]
        L1["📦 Offset 0: Pedido creado"]
        L2["📦 Offset 1: Pedido pagado"]
        L3["📦 Offset 2: Pedido enviado"]
        L4["📦 Offset 3: Pedido entregado"]
        L5["📦 Offset 4: ..."]
    end

    C_1["📥 Consumidor A<br/>(offset 2)"] --> L3
    C_2["📥 Consumidor B<br/>(offset 4)"] --> L5

    Note["💡 Cada consumidor lleva su propio offset.<br/>Puede leer desde el principio o desde donde se quedó."]

    style Log fill:#e8f5e9
    style C_1 fill:#e3f2fd
    style C_2 fill:#fff3e0
    style Note fill:#f5f5f5
```

### Ejemplo Node.js

```javascript
const { Kafka } = require('kafkajs');

// PRODUCTOR
const kafka = new Kafka({ clientId: 'miapp', brokers: ['localhost:9092'] });
const producer = kafka.producer();

async function enviarEvento() {
    await producer.connect();
    await producer.send({
        topic: 'pedidos',
        messages: [{ key: 'pedido-1', value: JSON.stringify({ id: 1, total: 100 }) }]
    });
}

// CONSUMIDOR
const consumer = kafka.consumer({ groupId: 'grupo-pagos' });

async function consumir() {
    await consumer.connect();
    await consumer.subscribe({ topic: 'pedidos', fromBeginning: true });

    await consumer.run({
        eachMessage: async ({ topic, partition, message }) => {
            console.log('Procesando:', message.value.toString());
            // ... lógica de negocio
        }
    });
}
```

### Kafka vs RabbitMQ

```mermaid
flowchart TB
    subgraph Comparativa ["⚖️ RabbitMQ vs Kafka"]
        RMQ["🐇 RabbitMQ"] --> R1["✅ Routing flexible<br/>(exchanges)"]
        RMQ --> R2["✅ Colas persistentes<br/>con ACKs"]
        RMQ --> R3["✅ UI de gestión"]
        RMQ --> R4["⚠️ Rendimiento medio<br/>(~10K msg/s)"]

        KFK["⚡ Kafka"] --> K1["✅ Alto rendimiento<br/>(~1M msg/s)"]
        KFK --> K2["✅ Retención por tiempo<br/>(reprocesar histórico)"]
        KFK --> K3["✅ Particionado y<br/>escalabilidad horizontal"]
        KFK --> K4["⚠️ Más complejo de operar"]
    end

    style Comparativa fill:#f5f5f5
    style RMQ fill:#fff3e0
    style KFK fill:#fce4ec
```

### ¿Cuál elegir?

```mermaid
flowchart TD
    Pregunta{"🔍 ¿Qué necesitas?"}

    Pregunta -->|"Colas de trabajo,<br/>comunicación entre servicios"| RMQ2["🐇 RabbitMQ<br/>Simple, fiable,<br/>maduro"]
    Pregunta -->|"Alto throughput,<br/>eventos, streaming"| KFK2["⚡ Kafka<br/>Millones de msg/s,<br/>retención, histórico"]
    Pregunta -->|"Bajo volumen,<br/>fácil de operar"| RMQ2
    Pregunta -->|"Pipeline de datos,<br/>logs, métricas, IoT"| KFK2

    style Pregunta fill:#e1f5fe
    style RMQ2 fill:#fff3e0
    style KFK2 fill:#fce4ec
```

### Tabla comparativa

| Característica           | RabbitMQ           | Kafka                |
| ------------------------ | ------------------ | -------------------- |
| **Modelo**               | Colas + Exchange   | Log particionado     |
| **Rendimiento**          | ~10K msg/s         | ~1M msg/s            |
| **Routing**              | Flexible (AMQP)    | Simple (tópico)      |
| **Persistencia**         | ACK + disco        | Retención por tiempo |
| **Consumidores**         | Comptien por cola  | Grupo particionado   |
| **Reprocesar**           | ❌ (se elimina)    | ✅ (retención)       |
| **Complejidad**          | Baja-Media         | Alta                 |
| **Caso típico**          | Microservicios     | Eventos, analytics   |

---

## Buenas prácticas

```mermaid
flowchart LR
    subgraph BestPractices ["✅ Buenas prácticas"]
        B1["Mensajes pequeños<br/>(< 1 MB)"]
        B2["ACK manual: confirmar<br/>solo si se procesó"]
        B3["DLQ para mensajes<br/>que fallan"]
        B4["Idempotencia: procesar<br/>el mismo mensaje 2 veces<br/>no debe causar problemas"]
        B5["Mensajes con schema<br/>(JSON Schema, Avro)"]
        B6["Monitoreo: colas,<br/>latencia, throughput"]
    end

    style BestPractices fill:#c8e6c9
```

---

## Resumen visual

```mermaid
graph TB
    Broker["🔌 Message Broker"] --> RMQ3["🐇 RabbitMQ<br/>Colas + AMQP<br/>Microservicios,<br/>tareas en segundo plano"]
    Broker --> KFK3["⚡ Kafka<br/>Log particionado<br/>Alto throughput,<br/>eventos, streaming"]

    Broker --> Patrones2["📐 Patrones"]
    Patrones2 --> Cola2["📋 Cola simple<br/>1:1"]
    Patrones2 --> PubSub2["📢 Pub/Sub<br/>1:N"]
    Patrones2 --> Stream2["🌊 Stream<br/>Orden + retención"]

    Broker --> When["🤔 RabbitMQ para 9/10 casos<br/>Kafka cuando necesitas<br/>escala + retención + eventos"]

    style Broker fill:#e1f5fe
    style RMQ3 fill:#fff3e0
    style KFK3 fill:#fce4ec
    style Patrones2 fill:#e8eaf6
    style When fill:#f5f5f5
```

> **Siguiente paso:** Instala RabbitMQ con Docker y crea un productor + consumidor. Luego prueba Kafka si necesitas alto rendimiento o eventos históricos.

## Relacionados:
- [[containerización]] #anterior 
- [[motores-de-busqueda]] #siguiente 