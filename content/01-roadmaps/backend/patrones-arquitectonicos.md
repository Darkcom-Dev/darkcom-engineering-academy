# Patrones Arquitectónicos

Los **patrones arquitectónicos** son soluciones probadas a problemas recurrentes de diseño de software. Definen la **estructura general** del sistema: cómo se organizan los componentes, cómo se comunican y cómo escalan.

```mermaid
flowchart TB
    PA["🏗️ Patrones Arquitectónicos"] --> Monolitico["🏢 Monolítico<br/>Todo en uno"]
    PA --> Microservicios["🧩 Microservicios<br/>Muchos pequeños"]
    PA --> SOA["🔗 SOA<br/>Servicios empresariales"]
    PA --> Serverless["⚡ Serverless<br/>Sin servidor que gestionar"]
    PA --> Mesh["🔀 Service Mesh<br/>Comunicación gestionada"]
    PA --> Twelve["📋 Twelve-Factor App<br/>Metodología SaaS"]

    style PA fill:#e1f5fe
    style Monolitico fill:#fff3e0
    style Microservicios fill:#c8e6c9
    style SOA fill:#fce4ec
    style Serverless fill:#e8eaf6
    style Mesh fill:#f3e5f5
    style Twelve fill:#fff9c4
```

---

## Monolítico

Un **monolito** es una aplicación donde **toda la funcionalidad** vive en un solo código base, se despliega como una sola unidad y corre en un solo proceso.

```mermaid
flowchart TB
    subgraph Monolith ["🏢 Arquitectura Monolítica"]
        UI["🖥️ Interfaz de Usuario"]
        API["🌐 API / Rutas"]
        Logic["🧠 Lógica de Negocio<br/>Usuarios │ Pedidos │ Pagos │ Productos"]
        DB["🗄️ Base de Datos"]
    end

    UI --> API --> Logic --> DB

    style Monolith fill:#fff3e0
    style UI fill:#e1f5fe
    style API fill:#ffe0b2
    style Logic fill:#ffcc80
    style DB fill:#fce4ec
```

### Ventajas y desventajas

| Ventajas                     | Desventajas                          |
| ---------------------------- | ------------------------------------ |
| ✅ Simple de desarrollar      | ❌ Escala todo o nada                |
| ✅ Fácil de desplegar         | ❌ Un bug puede tumbar todo          |
| ✅ Comunicación rápida        | ❌ Difícil de mantener al crecer     |
| ✅ Monitoreo simple           | ❌ Tecnología única forzada          |
| ✅ Ideal para empezar         | ❌ Despliegues lentos y riesgosos    |

> **¿Cuándo usarlo?** MVPs, proyectos pequeños, equipos chicos, startups en etapa inicial. **No asumas que necesitas microservicios.** La mayoría de proyectos deberían empezar como monolito.

---

## Microservicios

Los **microservicios** dividen la aplicación en **servicios pequeños, independientes y desplegables** cada uno con su propia lógica, BD y API.

```mermaid
flowchart TB
    subgraph Microservices ["🧩 Arquitectura de Microservicios"]
        Gateway["🚪 API Gateway"] --> Users["👤 Servicio Usuarios"]
        Gateway --> Orders["📦 Servicio Pedidos"]
        Gateway --> Payments["💳 Servicio Pagos"]
        Gateway --> Notifications["📧 Servicio Notificaciones"]
        Gateway --> Inventory["📋 Servicio Inventario"]

        Users --> DB1["🗄️ BD Usuarios"]
        Orders --> DB2["🗄️ BD Pedidos"]
        Payments --> DB3["🗄️ BD Pagos"]
        Inventory --> DB4["🗄️ BD Inventario"]

        Orders --> Broker["🔌 Message Broker"]
        Payments --> Broker
        Notifications --> Broker
        Inventory --> Broker
    end

    style Microservices fill:#c8e6c9
    style Gateway fill:#e1f5fe
    style Broker fill:#ffcc80
```

### Características clave

- **Despliegue independiente** — cada servicio se deploya sin afectar a otros
- **BD por servicio** — cada uno tiene su propia base de datos (desacoplamiento)
- **Comunicación vía red** — HTTP/REST, gRPC o mensajes (RabbitMQ, Kafka)
- **Escalado granular** — escalas solo el servicio que lo necesita
- **Tecnología heterogénea** — cada servicio puede usar su lenguaje/BD ideal

### El problema de la BD compartida

```mermaid
flowchart LR
    subgraph MonolithDB ["🚫 Monolito: BD compartida"]
        M1["Usuarios"] --> BD["🗄️ BD Única"]
        M2["Pedidos"] --> BD
        M3["Pagos"] --> BD
    end

    subgraph MicroDB ["✅ Microservicios: BD por servicio"]
        M4["Usuarios"] --> DB4b["🗄️ BD Usuarios"]
        M5["Pedidos"] --> DB5b["🗄️ BD Pedidos"]
        M6["Pagos"] --> DB6b["🗄️ BD Pagos"]
    end

    style MonolithDB fill:#ffcdd2
    style MicroDB fill:#c8e6c9
```

### Retos de microservicios

| Reto                | Descripción                                    |
| ------------------- | ---------------------------------------------- |
| **Complejidad**     | Múltiples servicios, despliegues, monitoreo    |
| **Comunicación**    | Latencia de red, fallos, reintentos            |
| **Consistencia**    | Transacciones distribuidas (eventual consistency) |
| **Testing**         | Tests de integración entre servicios           |
| **Observabilidad**  | Logs, métricas, traces distribuidos            |
| **DevOps**          | CI/CD, infraestructura, contenedores           |

> **¿Cuándo usarlos?** Equipos grandes (2+ por servicio), cuando partes del sistema escalan de forma diferente, cuando necesitas tecnologías diversas para distintas funcionalidades.

---

## SOA (Service-Oriented Architecture)

**SOA** es un estilo arquitectónico donde los servicios son **grandes, reutilizables y orquestados** a través de un bus empresarial (ESB). Fue el predecesor de microservicios.

```mermaid
flowchart TB
    subgraph SOA ["🔗 SOA - Service-Oriented Architecture"]
        App1["📱 App Cliente 1"]
        App2["📱 App Cliente 2"]

        ESB["🔌 ESB - Enterprise Service Bus"]
        Registry["📋 Service Registry"]

        S1["🏢 Servicio Clientes"]
        S2["🏢 Servicio Facturación"]
        S3["🏢 Servicio Inventario"]
    end

    App1 --> ESB
    App2 --> ESB
    ESB --> S1
    ESB --> S2
    ESB --> S3
    ESB --> Registry

    style SOA fill:#fce4ec
    style ESB fill:#ffcc80
```

### SOA vs Microservicios

| Característica | SOA                      | Microservicios            |
| -------------- | ------------------------ | ------------------------- |
| **Tamaño**     | Grandes servicios        | Pequeños servicios        |
| **Comunicación** | ESB (bus pesado)       | API directa / broker      |
| **BD**         | Compartida frecuentemente | Por servicio             |
| **Governance** | Centralizado             | Descentralizado           |
| **Despliegue** | Monolítico por servicio  | Independiente             |

---

## Serverless

**Serverless** (Funciones como Servicio / FaaS) ejecuta tu código en respuesta a eventos **sin que gestiones servidores**. Pagas solo por el tiempo de ejecución.

```mermaid
flowchart TB
    subgraph ServerlessArch ["⚡ Serverless"]
        Trigger1["📤 HTTP Request<br/>(API Gateway)"] --> Fn1["⚡ Función 1<br/>(Node.js)"]
        Trigger2["📥 S3 Upload"] --> Fn2["⚡ Función 2<br/>(Python)"]
        Trigger3["⏰ Cron / Schedule"] --> Fn3["⚡ Función 3<br/>(Go)"]
        Trigger4["📨 Queue Message"] --> Fn4["⚡ Función 4<br/>(Java)"]

        Fn1 --> S3["☁️ S3"]
        Fn2 --> DB5["☁️ DynamoDB"]
        Fn3 --> SQS["☁️ SQS"]
    end

    style ServerlessArch fill:#e8eaf6
    style Trigger1 fill:#e1f5fe
    style Trigger2 fill:#fff3e0
    style Trigger3 fill:#c8e6c9
    style Trigger4 fill:#fce4ec
```

### Ventajas y limitaciones

| Ventajas                       | Limitaciones                         |
| ------------------------------ | ------------------------------------ |
| ✅ Sin gestión de servidores    | ❌ Tiempo máximo de ejecución (15 min) |
| ✅ Escala automáticamente       | ❌ Cold starts (latencia inicial)     |
| ✅ Pago por uso (idle = $0)    | ❌ Difícil debugging local            |
| ✅ Escalamiento a cero          | ❌ Vendor lock-in                     |

> **¿Cuándo usarlo?** APIs con tráfico variable, procesamiento de eventos, tareas programadas, prototipos. AWS Lambda, Google Cloud Functions, Cloudflare Workers.

---

## Service Mesh

Un **Service Mesh** es una capa de infraestructura dedicada a gestionar la **comunicación entre microservicios** (descubrimiento, load balancing, retry, métricas, trazabilidad).

```mermaid
flowchart TB
    subgraph ServiceMesh ["🔀 Service Mesh"]
        subgraph Pod1 ["📦 Pod 1"]
            S1["🧩 Servicio A"]
            Proxy1["🔌 Proxy (sidecar)"]
        end

        subgraph Pod2 ["📦 Pod 2"]
            S2["🧩 Servicio B"]
            Proxy2["🔌 Proxy (sidecar)"]
        end

        subgraph Pod3 ["📦 Pod 3"]
            S3["🧩 Servicio C"]
            Proxy3["🔌 Proxy (sidecar)"]
        end

        Control["🎮 Control Plane<br/>(mTLS, policies, metrics)"]
    end

    Proxy1 --> Proxy2
    Proxy1 --> Proxy3
    Proxy2 --> Proxy3
    Control --> Proxy1
    Control --> Proxy2
    Control --> Proxy3

    style ServiceMesh fill:#f3e5f5
    style Control fill:#e1f5fe
    style Proxy1 fill:#ffcc80
    style Proxy2 fill:#ffcc80
    style Proxy3 fill:#ffcc80
```

### ¿Qué resuelve?

| Problema                    | Solución Service Mesh               |
| --------------------------- | ------------------------------------ |
| **mTLS entre servicios**    | Encriptación automática              |
| **Retry / Timeout**         | Políticas de resiliencia             |
| **Traza distribuida**       | OpenTelemetry integrado              |
| **Blue/Green, Canary**      | Enrutamiento de tráfico              |
| **Métricas por servicio**   | Prometheus + Grafana                 |

**Implementaciones:** Istio, Linkerd, Consul Connect, Envoy.

---

## Twelve-Factor App

**Twelve-Factor App** es una metodología para construir **aplicaciones SaaS** modernas, portables y escalables. Doce principios basados en la experiencia de Heroku.

```mermaid
flowchart TB
    Twelve2["📋 Twelve-Factor App"]

    Twelve2 --> F1["① Código base<br/>Un repo, múltiples deploys"]
    Twelve2 --> F2["② Dependencias<br/>Explicitas y aisladas"]
    Twelve2 --> F3["③ Configuración<br/>En variables de entorno"]
    Twelve2 --> F4["④ Backing services<br/>Recursos como adjuntos"]
    Twelve2 --> F5["⑤ Build, release, run<br/>Separación estricta"]
    Twelve2 --> F6["⑥ Procesos<br/>Sin estado (stateless)"]
    Twelve2 --> F7["⑦ Puerto bind<br/>Auto-contenido (export port)"]
    Twelve2 --> F8["⑧ Concurrencia<br/>Escalar con procesos"]
    Twelve2 --> F9["⑨ Desechabilidad<br/>Inicio/rápido, shutdown graceful"]
    Twelve2 --> F10["⑩ Paridad dev/prod<br/>Entornos lo más similares posible"]
    Twelve2 --> F11["⑪ Logs<br/>Como flujo de eventos"]
    Twelve2 --> F12["⑫ Admin processes<br/>Tareas como one-off"]

    style Twelve2 fill:#fff9c4
```

### Los 12 factores resumidos

| #  | Factor                        | Significado                                    |
| -- | ----------------------------- | ---------------------------------------------- |
| 1  | **Código base**               | Un repo por app, múltiples deploys (staging, prod) |
| 2  | **Dependencias**              | Declarar todo (package.json, requirements.txt) |
| 3  | **Configuración**             | Variables de entorno, no archivos en el código |
| 4  | **Backing services**          | BD, Redis, etc. son recursos adjuntos          |
| 5  | **Build, release, run**       | Separar build, release y ejecución             |
| 6  | **Procesos**                  | Stateless, no guardar estado local             |
| 7  | **Port binding**              | La app es auto-contenida (exporta puerto)      |
| 8  | **Concurrencia**              | Escalar horizontalmente con procesos           |
| 9  | **Desechabilidad**            | Startup rápido, shutdown graceful              |
| 10 | **Paridad dev/prod**          | Entornos lo más parecidos posible              |
| 11 | **Logs**                      | stdout, no archivos de log                     |
| 12 | **Admin processes**           | Migraciones, scripts como one-off procesos     |

---

# Diseño y Arquitectura

El **diseño arquitectónico** es el proceso de definir la estructura, componentes, interfaces y datos de un sistema para satisfacer requisitos funcionales y no funcionales.

## Principios fundamentales

```mermaid
flowchart TB
    Principios["🎯 Principios de Diseño"]

    Principios --> SOLID["SOLID<br/>Principios OOP"]
    Principios --> DRY["DRY<br/>Don't Repeat Yourself"]
    Principios --> KISS["KISS<br/>Keep It Simple, Stupid"]
    Principios --> YAGNI["YAGNI<br/>You Ain't Gonna Need It"]
    Principios --> Separation["🔀 Separación de<br/>Preocupaciones"]
    Principios --> Coupling["🔗 Bajo Acoplamiento<br/>Alta Cohesión"]

    SOLID --> S["SRP: Una razón para cambiar"]
    SOLID --> O["OCP: Abierto a extensión,<br/>cerrado a modificación"]
    SOLID --> L["LSP: Subtipos sustituibles"]
    SOLID --> I["ISP: Interfaces pequeñas"]
    SOLID --> D["DIP: Depender de abstracciones"]

    style Principios fill:#e1f5fe
```

### C4 Model para documentar arquitectura

El **C4 Model** es una forma jerárquica de documentar arquitectura de software en 4 niveles.

```mermaid
flowchart TB
    C4["📐 C4 Model"]

    C4 --> N1["🎯 Nivel 1: Contexto<br/>El sistema y sus usuarios"]
    C4 --> N2["🏗️ Nivel 2: Contenedores<br/>Apps, APIs, BD"]
    C4 --> N3["🧩 Nivel 3: Componentes<br/>Módulos internos"]
    C4 --> N4["📄 Nivel 4: Código<br/>Clases, interfaces"]

    N1 --> E1["Diagrama de Contexto"]
    N2 --> E2["Diagrama de Contenedores"]
    N3 --> E3["Diagrama de Componentes"]
    N4 --> E4["Diagrama de Clases"]

    style C4 fill:#e8f5e9
```

### Atributos de calidad (no funcionales)

```mermaid
graph TB
    Calidad["📊 Atributos de Calidad"] --> Rendimiento["⚡ Rendimiento<br/>Tiempo de respuesta, throughput"]
    Calidad --> Escalabilidad["📈 Escalabilidad<br/>Horizontal, vertical"]
    Calidad --> Disponibilidad["✅ Disponibilidad<br/>Uptime, SLA"]
    Calidad --> Seguridad["🔒 Seguridad<br/>Auth, encriptación"]
    Calidad --> Mantenibilidad["🔧 Mantenibilidad<br/>Código limpio, tests"]
    Calidad --> Confiabilidad["💪 Confiabilidad<br/>Resistencia a fallos"]
    Calidad --> Usabilidad["👤 Usabilidad<br/>Experiencia de usuario"]

    style Calidad fill:#f3e5f5
```

---

## Patrón de diseño de sistemas comunes

- **Proxy inverso** (Nginx) — balanceo de carga, SSL, caché
- **Colas** (RabbitMQ, Kafka) — comunicación asíncrona, desacoplamiento
- **Caché** (Redis) — acelerar consultas frecuentes
- **CDN** — contenido estático cerca del usuario
- **Read replicas** — separar lecturas de escrituras en BD
- **Sharding** — dividir BD horizontalmente
- **CQRS** — separar comandos (escritura) de consultas (lectura)
- **Event Sourcing** — almacenar eventos en lugar de estado actual

---

## Resumen visual

```mermaid
graph TB
    Arq["🏗️ Patrones Arquitectónicos"]
    Arq --> Mono2["🏢 Monolítico<br/>➕ Simple, rápido de empezar<br/>➖ Escala todo, acoplamiento"]
    Arq --> Micro2["🧩 Microservicios<br/>➕ Escala granular, independiente<br/>➖ Complejidad, red"]
    Arq --> SOA2["🔗 SOA<br/>➕ Reutilización empresarial<br/>➖ ESB pesado"]
    Arq --> Serverless2["⚡ Serverless<br/>➕ Sin gestión, pago por uso<br/>➖ Cold starts, timeouts"]
    Arq --> Mesh2["🔀 Service Mesh<br/>➕ Comunicación gestionada<br/>➖ Overhead operativo"]
    Arq --> Twelve3["📋 Twelve-Factor<br/>➕ Metodología SaaS probada<br/>➖ Requiere disciplina"]

    Arq --> Design["🎯 Diseño de Sistemas"]
    Design --> SOLID2["SOLID, DRY, KISS, YAGNI"]
    Design --> C42["C4 Model para documentar"]
    Design --> Patterns["Proxy, Colas, Caché,<br/>CDN, Sharding, CQRS"]

    style Arq fill:#e1f5fe
    style Design fill:#e8f5e9
    style Patterns fill:#c8e6c9
```

> **Siguiente paso:** Documenta la arquitectura de tu proyecto actual con C4 Model (empieza con el diagrama de contexto). Evalúa si tu arquitectura actual cumple con los Twelve-Factor App principles.

## Relacionados:
- [[motores-de-busqueda]] #anterior 
- [[data-en-tiempo-real]] #siguiente