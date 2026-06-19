# CI/CD

**CI/CD** (Continuous Integration / Continuous Delivery) es una práctica de desarrollo que automatiza la integración, prueba y despliegue del código. El objetivo: detectar errores rápido y entregar software con frecuencia y confianza.

```mermaid
flowchart LR
    Dev["👨‍💻 Desarrollador"] --> Code["✏️ Escribe código"]
    Code --> Push["📤 git push"]
    Push --> CI["⚙️ CI - Integración Continua<br/>Compila + Tests + Lint"]
    CI -->|"✅ Todo OK"| CD["🚀 CD - Entrega Continua<br/>Build + Deploy"]
    CD --> Prod["🌍 Producción"]

    CI -.->|"❌ Falla"| Fix["🔧 Corregir y repetir"]

    style Dev fill:#e1f5fe
    style Code fill:#fff3e0
    style Push fill:#c8e6c9
    style CI fill:#fce4ec
    style CD fill:#e8eaf6
    style Prod fill:#e8f5e9
    style Fix fill:#ffcdd2
```

### ¿Qué problema resuelve?

```mermaid
flowchart TB
    subgraph SinCI ["🚫 Sin CI/CD"]
        A1["👨‍💻 Dev 1: 'Ya terminé mi feature'"]
        A2["👩‍💻 Dev 2: 'Yo también'"]
        A3["😱 Día del merge: TODO explota"]
        A4["🕵️ '¿Quién rompió qué?'"]
        A5["😰 Miedito a desplegar"]
    end

    subgraph ConCI ["✅ Con CI/CD"]
        B1["👨‍💻 Dev 1: Push → CI corre tests"]
        B2["👩‍💻 Dev 2: Push → CI corre tests"]
        B3["❌ Si falla, se sabe al instante"]
        B4["✅ Merge seguro y frecuente"]
        B5["🚀 Deploy automático y confiable"]
    end

    style SinCI fill:#ffcdd2
    style ConCI fill:#c8e6c9
```

---

## CI: Integración Continua

**CI** ejecuta automáticamente **tests, linters y compilación** cada vez que haces push. Te dice si tu código rompió algo **en segundos o minutos**.

```mermaid
flowchart TB
    Push2["📤 git push / PR abierto"] --> Pipeline["⚡ Pipeline de CI"]

    Pipeline --> Checkout["📦 Checkout del código"]
    Checkout --> Deps["📥 Instalar dependencias"]
    Deps --> Lint["🔍 Linter<br/>(estilo y calidad)"]
    Deps --> TypeCheck["📐 Type check<br/>(TypeScript, mypy)"]
    Deps --> Tests["🧪 Tests<br/>✅ Unitarios<br/>✅ Integración"]
    Tests --> Build["📦 Build / Compilación"]

    Build --> Result{"¿Todo OK?"}
    Result -->|"✅ Sí"| Green["🟢 Pipeline verde"]
    Result -->|"❌ No"| Red["🔴 Pipeline rojo<br/>Notifica al equipo"]

    style Push2 fill:#e1f5fe
    style Pipeline fill:#fff3e0
    style Green fill:#c8e6c9
    style Red fill:#ffcdd2
```

### Ejemplo: GitHub Actions (Node.js)

```yaml
# .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Instalar dependencias
        run: npm ci

      - name: Linter
        run: npm run lint

      - name: Tests
        run: npm test

      - name: Build
        run: npm run build
```

### Ejemplo: GitLab CI

```yaml
# .gitlab-ci.yml
stages:
  - test
  - build

test:
  stage: test
  image: node:20
  script:
    - npm ci
    - npm run lint
    - npm test

build:
  stage: build
  image: node:20
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
```

---

## CD: Entrega/Despliegue Continuo

**CD** automatiza el despliegue del código validado a los entornos correspondientes (staging, producción).

```mermaid
flowchart LR
    CI_ok["🟢 CI pasa"] --> CD_Process["🚀 CD Pipeline"]

    CD_Process --> BuildArtifact["📦 Build:<br/>Binario / Docker / Bundle"]
    BuildArtifact --> DeployStaging["🧪 Deploy a Staging"]
    DeployStaging --> TestsExtra["🔬 Tests extra:<br/>E2E, integración, humo"]
    TestsExtra -->|"✅ OK"| DeployProd["🌍 Deploy a Producción"]
    TestsExtra -->|"❌ Falla"| Rollback["↩️ Rollback automático"]

    style CI_ok fill:#c8e6c9
    style CD_Process fill:#fff3e0
    style DeployStaging fill:#e3f2fd
    style DeployProd fill:#c8e6c9
    style Rollback fill:#ffcdd2
```

### CD: Delivery vs Deployment

```mermaid
flowchart LR
    subgraph Delivery ["📦 Continuous Delivery"]
        D1["✅ CI pasa"]
        D2["✅ Build automático"]
        D3["✅ Listo para deploy<br/>pero alguien da clic"]
        D4["👨‍💻 Manual approval"]
    end

    subgraph Deployment ["🚀 Continuous Deployment"]
        P1["✅ CI pasa"]
        P2["✅ Build automático"]
        P3["✅ Deploy automático<br/>a producción"]
        P4["🤖 Sin intervención humana"]
    end

    style Delivery fill:#fff3e0
    style Deployment fill:#c8e6c9
```

|                           | Continuous Delivery | Continuous Deployment |
| ------------------------- | ------------------- | --------------------- |
| **Despliegue a prod**     | Manual (clic)       | Automático            |
| **Riesgo**                | Menor (alguien revisa) | Mayor (totalmente automático) |
| **Velocidad**            | Rápido              | Inmediato             |
| **¿Para quién?**          | Equipos con releases programadas | Equipos con confianza en tests |

### Ejemplo: CD con GitHub Actions

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Build Docker image
        run: docker build -t miapp .

      - name: Push to registry
        run: docker push ghcr.io/miusuario/miapp:latest

      - name: Deploy to server
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /app
            docker-compose pull
            docker-compose up -d
```

---

## Estrategias de despliegue

```mermaid
flowchart TB
    subgraph Estrategias ["📐 Estrategias de deploy"]
        Rolling["🔄 Rolling Update<br/>Reemplaza instancias<br/>una por una<br/>✅ Sin downtime"]
        BlueGreen["🔵🟢 Blue-Green<br/>Dos entornos idénticos<br/>Cambio de tráfico instantáneo<br/>✅ Rollback inmediato"]
        Canary["🐤 Canary Release<br/>% pequeño de tráfico<br/>al nuevo código<br/>✅ Prueba con usuarios reales"]
    end

    style Estrategias fill:#f5f5f5
    style Rolling fill:#e3f2fd
    style BlueGreen fill:#c8e6c9
    style Canary fill:#fff3e0
```

### Blue-Green Deployment

```mermaid
flowchart LR
    subgraph Antes ["Antes del deploy"]
        LB["⚖️ Load Balancer"] --> Blue["🔵 Blue (v1)<br/>🟢 100% tráfico"]
        LB --> Green["🟢 Green (v2)<br/>0% tráfico"]
    end

    subgraph Durante ["Despliegue"]
        LB2["⚖️ Load Balancer"] --> Blue2["🔵 Blue (v1)<br/>🔀 Switcheando..."]
        LB2 --> Green2["🟢 Green (v2)<br/>🔀 Switcheando..."]
    end

    subgraph Despues ["Nuevo estable"]
        LB3["⚖️ Load Balancer"] --> Blue3["🔵 Blue (v1)<br/>0% tráfico (backup)"]
        LB3 --> Green3["🟢 Green (v2)<br/>100% tráfico"]
    end

    style Antes fill:#fff3e0
    style Durante fill:#fff9c4
    style Despues fill:#c8e6c9
```

---

## Pipeline completo

```mermaid
flowchart TB
    subgraph PipelineCompleto ["⚡ Pipeline CI/CD Completo"]
        direction TB

        Commit["📤 git push"] --> Checkout2["📦 Checkout"]
        Checkout2 --> Install["📥 npm ci"]
        Install --> Lint2["🔍 Lint"]
        Lint2 --> Test2["🧪 Tests unitarios"]
        Test2 --> TestInt["🔗 Tests integración"]
        TestInt --> Build2["📦 Build"]

        Build2 --> Dockerize["🐳 Dockerizar"]
        Dockerize --> PushRegistry["📤 Push a registry"]
        PushRegistry --> DeployStaging2["🧪 Deploy staging"]
        DeployStaging2 --> TestE2E["🧪 Tests E2E"]

        TestE2E -->|"✅"| Approve["👨‍💻 Aprobación manual"]
        Approve --> DeployProd2["🌍 Deploy producción"]
        DeployProd2 --> Monitor["📊 Monitoreo"]

        TestE2E -->|"❌"| Rollback2["↩️ Rollback"]
    end

    style PipelineCompleto fill:#f5f5f5
    style Commit fill:#e1f5fe
    style DeployProd2 fill:#c8e6c9
    style Rollback2 fill:#ffcdd2
    style Monitor fill:#e8eaf6
```

---

## Mejores prácticas

```mermaid
flowchart LR
    subgraph BestPractices ["✅ Buenas prácticas CI/CD"]
        P1["Commits pequeños<br/>y frecuentes"]
        P2["Tests que se ejecuten<br/>rápido (< 10 min)"]
        P3["Pipeline fallido<br/>= prioridad #1"]
        P4["Artefactos inmutables<br/>(mismo build en staging y prod)"]
        P5["Variables de entorno<br/>(secrets en el CI,<br/>no en el código)"]
        P6["Monitoreo post-deploy<br/>alertas si algo falla"]
    end

    style BestPractices fill:#c8e6c9
```

### Reglas de oro

1. **Nunca hagas push directo a main** — usa Pull Requests con CI que valide
2. **Pipeline rápido es pipeline bueno** — si tarda >15 min, la gente ignora los fallos
3. **Un solo comando para deploy** — `npm run deploy` o similar
4. **Rollback con un clic** — si puedes deployar, puedes revertir
5. **Inmutabilidad** — build una vez, deploya ese mismo artefacto en todos lados

---

## Resumen visual

```mermaid
graph TB
    CICD["⚙️ CI/CD"] --> CI2["🔄 Integración Continua"]
    CICD --> CD2["🚀 Entrega/Despliegue Continuo"]

    CI2 --> Commit2["📤 git push"]
    CI2 --> AutoTest["🧪 Tests + Lint + Build"]
    CI2 --> Feedback["📢 Feedback inmediato"]

    CD2 --> Staging2["🧪 Staging"]
    CD2 --> Prod2["🌍 Producción"]
    CD2 --> Strategies["📐 Estrategias: Blue-Green,<br/>Rolling, Canary"]

    CICD --> Tools2["🛠️ Herramientas"]
    Tools2 --> GA["🐙 GitHub Actions"]
    Tools2 --> GL["🦊 GitLab CI/CD"]
    Tools2 --> Jenkins["🏗️ Jenkins"]
    Tools2 --> Circle["⚪ CircleCI"]

    style CICD fill:#e1f5fe
    style CI2 fill:#fce4ec
    style CD2 fill:#e8eaf6
    style Tools2 fill:#f5f5f5
```

---

> **Siguiente paso:** Agrega CI a tu proyecto con GitHub Actions (el `.yml` que viste arriba funciona). Luego configura CD para que al hacer push a main se deploye automáticamente a staging.

## Relacionados:
- [[patrones-de-integracion]] #anterior 
- [[mas-acerca-de-bases-de-datos]] #siguiente 