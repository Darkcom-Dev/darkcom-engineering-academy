# Containerización

La **containerización** empaqueta una aplicación con todo lo que necesita (código, librerías, configuraciones) en un **contenedor** liviano y portátil que se ejecuta igual en cualquier entorno.

```mermaid
flowchart TB
    subgraph VM ["☁️ Máquina Virtual"]
        App1["📱 App 1"]
        Lib1["📚 Librerías"]
        GuestOS["🖥️ Guest OS"]
        Hypervisor["🖥️ Hypervisor"]
        HostOS["🖥️ Host OS"]
        Server["💻 Servidor"]
    end

    subgraph Container ["📦 Contenedor"]
        App2["📱 App 1"]
        Lib2["📚 Librerías"]
        DockerEngine["🐳 Docker Engine"]
        HostOS2["🖥️ Host OS"]
        Server2["💻 Servidor"]
    end

    style VM fill:#fce4ec
    style Container fill:#c8e6c9
```

| Característica     | Máquina Virtual         | Contenedor           |
| ------------------ | ----------------------- | -------------------- |
| **Peso**           | GB (SO completo)        | MB (solo app)        |
| **Inicio**         | Minutos                 | Segundos             |
| **Aislamiento**    | Completo (SO propio)    | Procesos del host    |
| **Consumo**        | Alto (cada VM su SO)    | Bajo (comparten SO)  |

---

## Docker

**Docker** es la plataforma de containerización más usada. Permite empaquetar, distribuir y ejecutar aplicaciones en contenedores de forma sencilla.

```mermaid
flowchart LR
    Dev["👨‍💻 Desarrollo"] --> Dockerfile["📄 Dockerfile<br/>(receta del contenedor)"]
    Dockerfile --> Image["📦 Imagen Docker<br/>(plantilla)"]
    Image --> Registry["☁️ Registry<br/>(Docker Hub / GHCR)"]
    Registry --> Server3["🖥️ Servidor"]
    Server3 --> Container3["📦 Contenedor en ejecución"]
    Container3 --> User3["👤 Usuario"]

    style Dev fill:#e1f5fe
    style Dockerfile fill:#fff3e0
    style Image fill:#c8e6c9
    style Registry fill:#e3f2fd
    style Server3 fill:#fce4ec
    style Container3 fill:#c8e6c9
```

### Dockerfile

Un **Dockerfile** es la receta para construir una imagen. Cada instrucción crea una **capa** (layer) que se cachea.

```dockerfile
# Usa una imagen base con Node.js
FROM node:20-alpine

# Directorio de trabajo
WORKDIR /app

# Copia package.json primero (aprovecha caché)
COPY package*.json ./
RUN npm ci --production

# Copia el código fuente
COPY . .

# Puerto que expone
EXPOSE 3000

# Comando al iniciar
CMD ["node", "server.js"]
```

```mermaid
flowchart TB
    subgraph Layers ["🧅 Capas de una imagen Docker"]
        L1["🟦 FROM node:20-alpine<br/>Capa base (SO + Node)"]
        L2["🟩 WORKDIR /app<br/>Capa de directorio"]
        L3["🟨 COPY package*.json<br/>Capa de dependencias"]
        L4["🟧 RUN npm ci<br/>Capa con node_modules"]
        L5["🟥 COPY . .<br/>Capa del código"]
        L6["🟪 CMD ['node', 'server.js']<br/>Capa de comando"]
    end

    Note["💡 Cada capa se cachea. Si solo cambia el código,<br/>las capas 1-4 se reusan → build más rápido."]

    style Layers fill:#f5f5f5
    style Note fill:#e8f5e9
```

### Comandos esenciales

```bash
# Construir imagen
docker build -t miapp:latest .

# Ejecutar contenedor
docker run -d -p 3000:3000 --name mi-app miapp:latest

# Ver contenedores
docker ps
docker ps -a  # incluye detenidos

# Ver logs
docker logs -f mi-app

# Detener y eliminar
docker stop mi-app
docker rm mi-app

# Ejecutar comando dentro del contenedor
docker exec -it mi-app sh
```

```mermaid
flowchart TB
    subgraph DockerCommands ["🐳 Comandos útiles"]
        Build["build → Crear imagen"]
        Run["run → Crear y ejecutar contenedor"]
        PS["ps → Listar contenedores"]
        Logs["logs → Ver logs"]
        Exec["exec → Entrar al contenedor"]
        Stop["stop → Detener"]
        RM["rm → Eliminar contenedor"]
        Images["images → Listar imágenes"]
        Pull["pull → Descargar imagen"]
        Push["push → Subir imagen"]
    end

    style DockerCommands fill:#e3f2fd
```

### Docker Compose

Para aplicaciones con múltiples servicios (app + BD + Redis), **Docker Compose** orquesta todo con un archivo YAML.

```yaml
# docker-compose.yml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgres://user:pass@db:5432/miapp
    depends_on:
      - db

  db:
    image: postgres:16
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=pass

volumes:
  pgdata:
```

```bash
# Iniciar todos los servicios
docker compose up -d

# Ver logs
docker compose logs -f

# Detener
docker compose down
```

### Ventajas de Docker

| Ventaja               | Explicación                                    |
| --------------------- | ---------------------------------------------- |
| **Portabilidad**      | "Funciona en mi máquina" → funciona en todas   |
| **Aislamiento**       | Cada app con sus dependencias, sin conflictos  |
| **Reproducibilidad**  | Misma imagen → mismo comportamiento siempre    |
| **Escalabilidad**     | Múltiples contenedores = múltiples instancias  |
| **Eficiencia**        | Comparte SO, solo empaqueta la app             |

---

## LXC (Linux Containers)

**LXC** es una tecnología de containerización a nivel de **sistema operativo** que permite ejecutar entornos Linux completos y aislados. Es más cercano a una VM que Docker, pero sin el overhead de un hypervisor.

```mermaid
flowchart TB
    subgraph ComparativaLXC ["⚖️ Docker vs LXC"]
        Docker2["🐳 Docker"] --> DockerUse["📦 Un proceso por contenedor<br/>App + dependencias<br/>Ideal para microservicios"]
        LXC2["📦 LXC"] --> LXCUse["🖥️ Sistema completo<br/>init, servicios, ssh<br/>Ideal para entornos tipo VM"]
    end

    style ComparativaLXC fill:#f5f5f5
    style Docker2 fill:#e3f2fd
    style LXC2 fill:#fff3e0
```

### LXC vs Docker

| Característica       | Docker                     | LXC                         |
| -------------------- | -------------------------- | --------------------------- |
| **Propósito**        | Una app por contenedor     | Un sistema completo         |
| **Inicio**           | Segundos                   | Segundos (como Docker)      |
| **Init system**      | No (solo el proceso)       | Sí (systemd, OpenRC)        |
| **Caso típico**      | Microservicios, APIs       | Entornos de desarrollo, CI  |

---

# Orquestación de contenedores

La **orquestación** gestiona automáticamente el ciclo de vida de los contenedores: despliegue, escalado, redes, balanceo, actualizaciones y recuperación ante fallos.

```mermaid
flowchart TB
    subgraph Orquestacion ["🎼 Orquestación resuelve"]
        Scale["📈 Escalar: 1 → 10 instancias"]
        Health["❤️ Salud: reiniciar si falla"]
        Network["🌐 Red: comunicación entre servicios"]
        LB["⚖️ Balanceo: distribuir tráfico"]
        Update["🔄 Rolling update: sin downtime"]
        Config["🔧 Config: variables de entorno, secretos"]
    end

    style Orquestacion fill:#e8eaf6
```

---

## Kubernetes

**Kubernetes (K8s)** es el orquestador de contenedores más usado. Automatiza el despliegue, escalado y operación de contenedores en clusters de servidores.

```mermaid
flowchart TB
    subgraph Cluster ["☸️ Cluster Kubernetes"]
        subgraph Master ["🎯 Master Node (Control Plane)"]
            API["API Server"]
            Scheduler["Scheduler"]
            Controller["Controller Manager"]
            etcd["etcd (base de datos)"]
        end

        subgraph Workers ["💻 Worker Nodes"]
            N1["Node 1"] --> Pod1["📦 Pod: App"]
            N1 --> Pod2["📦 Pod: App"]
            N2["Node 2"] --> Pod3["📦 Pod: App"]
            N2 --> Pod4["📦 Pod: App"]
        end

        Master --> Workers
    end

    User2["👤 kubectl"] --> API

    style Cluster fill:#f5f5f5
    style Master fill:#e3f2fd
    style Workers fill:#ffe0b2
```

### Conceptos clave

```mermaid
flowchart TB
    K8s["☸️ Kubernetes"] --> Pod["📦 Pod<br/>Unidad mínima<br/>1+ contenedores"]
    K8s --> Deployment["🚀 Deployment<br/>Declara el estado deseado<br/>(réplicas, actualizaciones)"]
    K8s --> Service["🌐 Service<br/>IP estable + balanceo<br/>para acceder a pods"]
    K8s --> ConfigMap["🔧 ConfigMap<br/>Configuración<br/>no sensible"]
    K8s --> Secret["🔐 Secret<br/>Configuración<br/>sensible"]
    K8s --> Ingress["🚪 Ingress<br/>HTTP externo<br/>hacia servicios"]
    K8s --> Volume["💾 Volume<br/>Almacenamiento<br/>persistente"]

    style K8s fill:#e1f5fe
    style Pod fill:#c8e6c9
    style Deployment fill:#fff3e0
    style Service fill:#fce4ec
    style ConfigMap fill:#e8eaf6
    style Secret fill:#f3e5f5
    style Ingress fill:#fff9c4
    style Volume fill:#e8f5e9
```

### Ejemplo: Deployment + Service

```yaml
# deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: miapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: miapp
  template:
    metadata:
      labels:
        app: miapp
    spec:
      containers:
        - name: app
          image: miapp:latest
          ports:
            - containerPort: 3000
---
# service.yml
apiVersion: v1
kind: Service
metadata:
  name: miapp-svc
spec:
  selector:
    app: miapp
  ports:
    - port: 80
      targetPort: 3000
  type: LoadBalancer
```

```bash
# Aplicar configuración
kubectl apply -f deployment.yml
kubectl apply -f service.yml

# Ver estado
kubectl get pods
kubectl get deployments
kubectl get services

# Escalar
kubectl scale deployment miapp --replicas=5

# Rolling update (cambiar imagen)
kubectl set image deployment/miapp app=miapp:v2

# Logs
kubectl logs -l app=miapp
```

### Auto-escalado

```yaml
# Horizontal Pod Autoscaler: escala según CPU
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: miapp-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: miapp
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

```mermaid
flowchart TB
    subgraph AutoScale ["📈 Auto-escalado"]
        Normal["⚖️ 3 pods<br/>CPU 30%"] --> Spike["📈 Pico de tráfico<br/>CPU 85%"]
        Spike --> ScaleUp["🚀 Escala a 8 pods"]
        ScaleUp --> Distribute["✅ Carga distribuida"]
        Distribute --> Drop["📉 Tráfico baja<br/>CPU 30%"]
        Drop --> ScaleDown["🔄 Escala a 3 pods"]
    end

    style AutoScale fill:#fff9c4
    style Normal fill:#c8e6c9
    style ScaleUp fill:#e3f2fd
    style Distribute fill:#c8e6c9
    style Drop fill:#fff3e0
    style ScaleDown fill:#fce4ec
```

---

## Docker vs Kubernetes

```mermaid
flowchart TB
    Pregunta{"🔍 ¿Qué necesitas?"}

    Pregunta -->|"App simple, desarrollo local"| DockerOnly["🐳 Docker Compose<br/>Un servidor, pocos servicios"]
    Pregunta -->|"Equipo pequeño,<br/>despliegue sencillo"| DockerSwarm["🐳 Docker Swarm<br/>Orquestación simple"]
    Pregunta -->|"Producción robusta,<br/>escalabilidad, empresas"| K8s2["☸️ Kubernetes<br/>El estándar de la industria"]
    Pregunta -->|"Serverless, sin<br/>gestionar servidores"| CloudRun["☁️ Cloud Run / Fargate<br/>Sin orquestación manual"]

    style Pregunta fill:#e1f5fe
    style DockerOnly fill:#c8e6c9
    style DockerSwarm fill:#fff3e0
    style K8s2 fill:#e3f2fd
    style CloudRun fill:#e8f5e9
```

### Tabla comparativa

| Característica       | Docker Compose       | Docker Swarm       | Kubernetes             |
| -------------------- | -------------------- | ------------------ | ---------------------- |
| **Complejidad**      | Baja                | Media              | Alta                   |
| **Instalación**      | Incluida en Docker  | Incluida en Docker | Requiere setup         |
| **Escalabilidad**    | Un servidor         | Multi-servidor     | Multi-servidor masivo  |
| **Auto-recuperación**| ❌                  | ✅                 | ✅                     |
| **Rolling update**   | ❌                  | ✅                 | ✅                     |
| **Ecosistema**       | Pequeño             | Limitado           | Gigante (CNCF)         |
| **¿Para quién?**     | Dev local           | Equipos pequeños   | Producción profesional |

---

## Resumen visual

```mermaid
graph TB
    Containerization["📦 Containerización"]

    Containerization --> Docker3["🐳 Docker<br/>Empaquetar apps"]

    Docker3 --> Dockerfile2["📄 Dockerfile<br/>Receta de la imagen"]
    Docker3 --> Compose2["📋 Docker Compose<br/>Multi-servicio local"]
    Docker3 --> Registry2["☁️ Registry<br/>Distribuir imágenes"]

    Containerization --> LXC3["📦 LXC<br/>Contenedores tipo VM"]

    Containerization --> Orchestration["🎼 Orquestación"]

    Orchestration --> Swarm2["🐳 Docker Swarm<br/>Orquestación simple"]
    Orchestration --> K8s3["☸️ Kubernetes<br/>Orquestación profesional"]

    K8s3 --> Pods2["📦 Pods"]
    K8s3 --> Deploy2["🚀 Deployments"]
    K8s3 --> Services2["🌐 Services"]
    K8s3 --> HPA2["📈 Auto-escalado"]

    style Containerization fill:#e1f5fe
    style Docker3 fill:#e3f2fd
    style LXC3 fill:#fff3e0
    style Orchestration fill:#fce4ec
    style K8s3 fill:#c8e6c9
    style Swarm2 fill:#ffe0b2
```

---

> **Siguiente paso:** Crea un Dockerfile para tu proyecto backend. Luego añade un `docker-compose.yml` con tu app + PostgreSQL. Finalmente, si te sientes ambicioso, despliega en Kubernetes con Minikube local.

## Relacionados:
- [[testing]] #anterior 
- [[broker-de-mensajes]] #siguiente 