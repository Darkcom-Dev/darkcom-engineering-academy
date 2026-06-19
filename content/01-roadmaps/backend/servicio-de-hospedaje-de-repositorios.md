# Servicio de Alojamiento de Repositorios

Los servicios de alojamiento de repositorios son plataformas que almacenan tu código en la nube y añaden herramientas de colaboración: revisión de código, CI/CD, gestión de issues, wikis y más.

```mermaid
flowchart TB
    Git["🐙 Git<br/>(sistema local)"] --> Plataformas["☁️ Plataformas de alojamiento"]
    Plataformas --> GitHub["🐙 GitHub"]
    Plataformas --> GitLab["🦊 GitLab"]
    Plataformas --> Bitbucket["🔵 Bitbucket"]

    subgraph Features ["🔧 Funcionalidades comunes"]
        Repos["📦 Repositorios remotos"]
        PRs["🔀 Pull/Merge Requests"]
        Issues["📋 Issues y Projects"]
        CI_CD["⚙️ CI/CD Integrado"]
        Wiki["📝 Wikis"]
        Actions["🤖 Automatizaciones"]
    end

    GitHub --> Features
    GitLab --> Features

    style Git fill:#f5f5f5
    style Plataformas fill:#e3f2fd
    style Features fill:#e8f5e9
```

---

## GitHub

GitHub es la plataforma más grande del mundo para alojar código. Propiedad de Microsoft, alberga más de **100 millones de repositorios** y es el hogar de proyectos como React, Vue, Node.js, y el kernel de Linux.

```mermaid
graph LR
    subgraph GitHubFeatures ["🐙 GitHub"]
        A["📦 Repos privados<br/>y públicos"]
        B["🔀 Pull Requests<br/>con revisión"]
        C["🤖 GitHub Actions<br/>(CI/CD)"]
        D["📋 Issues +<br/>Projects"]
        E["🌐 GitHub Pages<br/>(hosting gratis)"]
        F["🛡️ Dependabot<br/>(seguridad)"]
        G["👥 Discussions<br/>(comunidad)"]
        H["🧑‍💻 Codespaces<br/>(IDE en la nube)"]
    end

    style GitHubFeatures fill:#fff3e0
```

### Pull Request: el corazón de la colaboración en GitHub

```mermaid
sequenceDiagram
    participant Dev as 👨‍💻 Desarrollador
    participant Fork as 🍴 Fork (tu copia)
    participant Branch as 🌿 feature/nueva-funcion
    participant GH as 🐙 GitHub
    participant Repo as 📦 Repo original

    Dev->>Fork: ① Hace fork del repo
    Dev->>Branch: ② git switch -c feature/login
    Dev->>Branch: ③ Trabaja y hace commits
    Dev->>Fork: ④ git push origin feature/login
    Dev->>Repo: ⑤ Abre Pull Request
    Repo->>Dev: ⑥ El dueño revisa el código
    Note over Repo,Dev: Discusión, sugerencias, cambios
    Repo->>Repo: ⑦ Merge a main ✅
    Repo->>Dev: ⑧ ¡Tu código está en producción!
```

### GitHub Actions: flujo típico

```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm test
```

```mermaid
flowchart LR
    A["📤 git push"] --> B["⚡ Action se dispara"]
    B --> C["📦 Checkout código"]
    C --> D["🔧 Instalar dependencias"]
    D --> E["🧪 Ejecutar tests"]
    E --> F{"¿Tests pasan?"}
    F -->|"✅ Sí"| G["✅ Green check"]
    F -->|"❌ No"| H["❌ Red cross"]
    G --> I["🎉 Listo para deploy"]
    H --> J["🔔 Notifica al equipo"]

    style A fill:#e1f5fe
    style B fill:#fff3e0
    style C fill:#e8f5e9
    style D fill:#fce4ec
    style E fill:#f3e5f5
    style F fill:#fff9c4
    style G fill:#c8e6c9
    style H fill:#ffcdd2
```

### Comandos esenciales para trabajar con GitHub

```bash
# Clonar un repositorio
git clone https://github.com/usuario/repo.git

# Conectar repo local a GitHub
git remote add origin https://github.com/usuario/repo.git

# Subir código por primera vez
git branch -M main
git push -u origin main

# Hacer fork de un repo (desde la web) y luego:
git clone https://github.com/tu-usuario/repo-forkeado.git
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git merge upstream/main
```

### Ventajas de GitHub

| Característica       | Descripción breve                           |
| -------------------- | ------------------------------------------- |
| **Comunidad**        | La red social de código más grande del mundo |
| **GitHub Actions**   | CI/CD integrado sin necesidad de terceros   |
| **GitHub Pages**     | Hosting gratuito para sitios estáticos       |
| **Codespaces**       | Entorno de desarrollo en la nube            |
| **Dependabot**       | Seguridad automatizada de dependencias      |
| **Projects**         | Gestión de proyectos tipo Kanban            |

---

## GitLab

GitLab es una plataforma completa de DevOps **open source**. A diferencia de GitHub, ofrece CI/CD integrado desde el inicio y permite auto-hospedaje (self-hosted) en tu propio servidor.

```mermaid
graph LR
    subgraph GitLabFeatures ["🦊 GitLab"]
        A["📦 Repos ilimitados<br/>públicos y privados"]
        B["🔀 Merge Requests<br/>con revisión"]
        C["⚙️ GitLab CI/CD<br/>(integrado nativo)"]
        D["📋 Issues +<br/>Epics + Milestones"]
        E["🛡️ Auto DevOps<br/>(configuración automática)"]
        F["🔐 Container Registry<br/>integrado"]
        G["📊 Value Stream<br/>Analytics"]
        H["🏠 Self-hosted<br/>(tu propio servidor)"]
    end

    style GitLabFeatures fill:#fce4ec
```

### La diferencia clave: CI/CD nativo

Mientras que en GitHub Actions es un añadido posterior, GitLab nació con **CI/CD en el núcleo**. El archivo `.gitlab-ci.yml` se define en la raíz del repo y GitLab lo ejecuta automáticamente en cada push.

```yaml
stages:
  - test
  - build
  - deploy

test:
  stage: test
  script:
    - npm install
    - npm test

build:
  stage: build
  script:
    - npm run build

deploy:
  stage: deploy
  script:
    - rsync -avz ./dist/ usuario@servidor:/var/www/
```

```mermaid
flowchart LR
    A["📤 git push"] --> B["⚡ .gitlab-ci.yml<br/>se ejecuta"]
    B --> C["🧪 Stage: test"]
    C -->|"✅ pasa"| D["📦 Stage: build"]
    D -->|"✅ pasa"| E["🚀 Stage: deploy"]
    E --> F["🎉 En producción!"]

    style A fill:#e1f5fe
    style B fill:#fff3e0
    style C fill:#c8e6c9
    style D fill:#e3f2fd
    style E fill:#ffe0b2
    style F fill:#f8bbd0
```

### Merge Request en GitLab

```mermaid
sequenceDiagram
    participant Dev as 👨‍💻 Desarrollador
    participant Repo as 🦊 GitLab
    participant Pipeline as ⚙️ CI Pipeline
    participant Reviewer as 👀 Revisor

    Dev->>Repo: ① git push origin feature
    Repo->>Pipeline: ② CI se ejecuta automáticamente
    Pipeline-->>Dev: ③ Resultado: ✅ tests pasan
    Dev->>Repo: ④ Abre Merge Request
    Repo->>Reviewer: ⑤ Asigna revisor
    Reviewer->>Repo: ⑥ Comentarios y aprobación
    Repo->>Repo: ⑦ Merge a main ✅
```

### Auto DevOps

GitLab puede **configurar todo automáticamente**: detecta el lenguaje, crea el pipeline, construye la imagen Docker, la sube al Container Registry y la despliega. Sin escribir una línea de YAML.

---

## GitHub vs GitLab: comparativa

```mermaid
flowchart TB
    subgraph Comparativa ["⚖️ GitHub vs GitLab"]
        direction TB

        GH["🐙 GitHub"] --> GH1["✅ Comunidad más grande"]
        GH --> GH2["✅ GitHub Actions<br/>(potente pero aparte)"]
        GH --> GH3["✅ GitHub Pages<br/>(hosting estático)"]
        GH --> GH4["✅ Copilot integrado<br/>(AI pair programming)"]
        GH --> GH5["⚠️ Solo cloud<br/>(no self-hosted gratuito)"]

        GL["🦊 GitLab"] --> GL1["✅ CI/CD nativo<br/>(desde el diseño)"]
        GL --> GL2["✅ Self-hosted<br/>(control total de datos)"]
        GL --> GL3["✅ Auto DevOps<br/>(configuración automática)"]
        GL --> GL4["✅ Container Registry<br/>integrado"]
        GL --> GL5["✅ Más features en<br/>versión gratuita"]
    end

    style Comparativa fill:#f5f5f5
    style GH fill:#fff3e0
    style GL fill:#fce4ec
```

### Tabla comparativa

| Característica              | GitHub                        | GitLab                        |
| --------------------------- | ----------------------------- | ----------------------------- |
| **Tipo**                    | Cloud (GitHub.com)            | Cloud + Self-hosted           |
| **Repos privados gratis**   | Ilimitados                    | Ilimitados                    |
| **CI/CD**                   | GitHub Actions (añadido)      | Nativo (GitLab CI/CD)         |
| **Auto DevOps**             | No                            | Sí                            |
| **Container Registry**      | GitHub Packages               | Nativo                        |
| **Pages / Hosting**         | GitHub Pages (estático)       | GitLab Pages (estático)       |
| **Modelo de negocio**       | Freemium                      | Open Source + Enterprise      |
| **AI integrado**            | Copilot                       | GitLab Duo                    |

---

## ¿Cuál elegir?

```mermaid
flowchart TD
    P{¿Qué necesitas?}

    P -->|"Comunidad y visibilidad"| GH1["🐙 GitHub<br/>El estándar de la industria<br/>Ideal para open source"]
    P -->|"Control total de datos"| GL1["🦊 GitLab self-hosted<br/>Para empresas que necesitan<br/>cumplimiento y privacidad"]
    P -->|"CI/CD potente sin configurar"| GL2["🦊 GitLab<br/>Auto DevOps + CI nativo"]
    P -->|"Portfolio / CV"| GH2["🐙 GitHub<br/>Tu perfil es tu currículum"]
    P -->|"Equipo pequeño / startup"| GH3["🐙 GitHub<br/>Comunidad, Actions, Pages"]

    style P fill:#e1f5fe
    style GH1 fill:#fff3e0
    style GH2 fill:#fff3e0
    style GH3 fill:#fff3e0
    style GL1 fill:#fce4ec
    style GL2 fill:#fce4ec
```

> **Ambos son excelentes.** Si recién empiezas, GitHub te da la comunidad más grande y visibilidad. Si necesitas control total o CI/CD nativo, GitLab es imbatible. Muchos equipos usan GitHub para desarrollo y GitLab auto-hospedado para despliegues internos.

## Relacionados:
- [[sistema-de-control-de-versiones]] #anterior 
- [[bases-de-datos-relacionales]] #siguiente 