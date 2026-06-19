# Sistema de Control de Versiones

Un **sistema de control de versiones** (VCS) registra los cambios en tus archivos a lo largo del tiempo, permitiéndote volver a versiones anteriores, experimentar sin miedo y colaborar con otros desarrolladores.

```mermaid
flowchart LR
    subgraph SinGit ["🚫 Sin control de versiones"]
        D1["proyecto-final.js"]
        D2["proyecto-final2.js"]
        D3["proyecto-final-final.js"]
        D4["proyecto-definitivo.js"]
    end

    subgraph ConGit ["✅ Con Git"]
        C1["📦 Repositorio"]
        C2["✅ commit 1"]
        C3["✅ commit 2"]
        C4["✅ commit 3"]
        C1 --> C2 --> C3 --> C4
        C2 -.->|"⬅️ Volver"| C1
    end

    style SinGit fill:#ffcdd2
    style ConGit fill:#c8e6c9
```

---

## Git

Git es el sistema de control de versiones más usado del mundo. Es **distribuido** (cada desarrollador tiene una copia completa del historial), **rápido** y **confiable**.

### Conceptos fundamentales

```mermaid
flowchart TB
    subgraph Estados ["📌 Estados de un archivo en Git"]
        Modified["✏️ Modified<br/>(modificado)"]
        Staged["📋 Staged<br/>(preparado)"]
        Committed["✅ Committed<br/>(guardado)"]
    end

    subgraph Areas ["🗂️ Áreas de Git"]
        Working["📁 Working Directory<br/>(tus archivos)"]
        Staging["📋 Staging Area<br/>(index)"]
        Repo["🗄️ Repositorio<br/>(.git)"]
    end

    Working -->|"git add"| Staging
    Staging -->|"git commit"| Repo
    Repo -->|"git checkout"| Working

    style Estados fill:#e3f2fd
    style Areas fill:#fff3e0
```

### Flujo de trabajo básico

```mermaid
flowchart LR
    A["📁 Working Directory<br/>Tus archivos"] -->|"1. git add"| B["📋 Staging Area<br/>Preparas cambios"]
    B -->|"2. git commit"| C["🗄️ Repositorio<br/>Guardas cambios"]
    C --> D["🌍 Remoto (GitHub)<br/>"]

    E["🌍 Remoto"] -->|"git pull / git fetch"| A

    style A fill:#bbdefb
    style B fill:#ffe0b2
    style C fill:#c8e6c9
    style D fill:#f8bbd0
    style E fill:#f8bbd0
```

---

## Comandos esenciales

### Configuración inicial (una sola vez)

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

### Crear o clonar un repositorio

| Comando                          | Qué hace                          |
| -------------------------------- | --------------------------------- |
| `git init`                       | Inicia un repo en la carpeta actual |
| `git clone <url>`                | Clona un repo remoto             |

### Trabajo diario

| Comando                        | Qué hace                              |
| ------------------------------ | ------------------------------------- |
| `git status`                   | Muestra el estado de tus archivos     |
| `git add <archivo>`            | Prepara un archivo para commit        |
| `git add .`                    | Prepara **todos** los archivos        |
| `git commit -m "mensaje"`      | Guarda los cambios preparados         |
| `git log`                      | Muestra el historial de commits       |
| `git log --oneline`            | Historial resumido (1 línea por commit) |
| `git diff`                     | Muestra cambios sin preparar          |
| `git diff --staged`            | Muestra cambios preparados            |

### Sincronizar con remoto

| Comando                        | Qué hace                              |
| ------------------------------ | ------------------------------------- |
| `git remote add origin <url>`  | Conecta tu repo local a uno remoto    |
| `git push origin main`         | Sube tus commits al remoto            |
| `git pull origin main`         | Baja y fusiona cambios del remoto     |
| `git fetch`                    | Baja cambios **sin fusionarlos**      |

### Ramas (branches)

```bash
git branch nombre-rama      # Crea una rama
git switch nombre-rama      # Cambia a una rama (o git checkout)
git switch -c nombre-rama   # Crea y cambia en un paso
git branch -d nombre-rama   # Elimina una rama
```

---

## Ramas (Branches)

Las ramas permiten trabajar en **múltiples versiones del proyecto en paralelo** sin afectar la versión estable.

```mermaid
gitGraph
    commit id: "Inicio"
    commit id: "Login"
    branch feature-nav
    checkout feature-nav
    commit id: "Nav bar"
    commit id: "Responsive"
    checkout main
    commit id: "Footer"
    merge feature-nav id: "Merge nav"
    commit id: "Deploy v1"
```

### Estrategia de ramas típica

```
main          → Código en producción (estable)
├── develop   → Integración de características
│   ├── feature/login   → Trabajo en login
│   ├── feature/api     → Trabajo en API
│   └── hotfix/crash    → Parche urgente
```

| Rama     | Propósito                                  |
| -------- | ------------------------------------------ |
| `main`   | Producción, solo commits estables          |
| `develop`| Integración de features en desarrollo      |
| `feature/*` | Cada nueva funcionalidad en su rama    |
| `hotfix/*`  | Parches urgentes a producción          |

---

## Resolución de conflictos

Cuando dos personas modifican la misma línea del mismo archivo, Git no sabe cuál conservar.

```mermaid
flowchart LR
    A["👤 Tú modificas línea 5"] --> C["💥 Conflicto"]
    B["👤 Otro modifica línea 5<br/>y hace push"] --> C
    C --> D["🔧 Resuelves manualmente"]
    D --> E["✅ git add + git commit"]

    style A fill:#bbdefb
    style B fill:#ffe0b2
    style C fill:#ffcdd2
    style D fill:#fff3e0
    style E fill:#c8e6c9
```

```bash
# Al hacer pull aparece un conflicto
# Git marca las zonas en conflicto:

<<<<<<< HEAD
console.log("versión tuya");
=======
console.log("versión del otro");
>>>>>>> feature-otro

# Editas el archivo, dejas lo correcto
# Y luego:
git add archivo-resuelto.js
git commit -m "Resuelve conflicto en archivo-resuelto.js"
```

---

## Comandos útiles para emergencias

```bash
# Deshacer cambios sin preparar
git restore <archivo>

# Sacar archivos del staging
git restore --staged <archivo>

# Modificar el último commit (mensaje o contenido)
git commit --amend -m "Nuevo mensaje"

# Guardar cambios temporales (sin commitear)
git stash
git stash pop   # Recuperar cambios guardados

# Ver quién escribió cada línea de un archivo
git blame <archivo>

# Historial gráfico
git log --graph --oneline --all
```

---

## Flujo de trabajo completo (día típico)

```mermaid
flowchart TB
    A["🌅 Inicio del día<br/>git pull"] --> B["🌱 Crear rama feature<br/>git switch -c feature/login"]
    B --> C["✏️ Editar archivos<br/>Working Directory"]
    C --> D["📋 Preparar cambios<br/>git add ."]
    D --> E["✅ Hacer commit<br/>git commit -m 'Add login'"]
    E --> F{"¿Feature completa?"}
    F -->|"No"| C
    F -->|"Sí"| G["🌍 Subir rama<br/>git push -u origin feature/login"]
    G --> H["🔀 Abrir Pull Request<br/>(GitHub/GitLab)"]
    H --> I["👀 Revisión de código"]
    I --> J["✅ Merge a main y<br/>git pull en local"]
```

---

## Resumen visual: Git en una imagen

```mermaid
graph TB
    RC["🆕 git init / clone"] --> WD["📁 Working Directory"]
    WD -->|"git add"| SA["📋 Staging Area"]
    SA -->|"git commit"| LH["🗄️ Local Repo"]
    LH -->|"git push"| RH["🌍 Remote Repo"]
    RH -->|"git pull"| WD

    LH --> BR["🌿 git branch / switch"]
    BR --> MERGE["🔀 git merge"]

    RH --> PR["🔄 Pull Request"]

    style RC fill:#e1f5fe
    style WD fill:#fff3e0
    style SA fill:#ffe0b2
    style LH fill:#c8e6c9
    style RH fill:#f8bbd0
    style BR fill:#e8f5e9
    style MERGE fill:#f3e5f5
    style PR fill:#e1f5fe
```

### Reglas de oro

1. **Commits pequeños y frecuentes** — cada commit debe ser una unidad lógica
2. **Mensajes claros** — explica **qué** y **por qué**, no cómo
3. **Una rama por feature** — no trabajes en `main`
4. **Nunca forces push** (`--force`) en ramas compartidas
5. **Revisa antes de commitear** — `git diff` y `git status` son tus amigos
6. **Siempre haz pull antes de empezar a trabajar**

---

> **Siguiente paso:** Practica con [Learn Git Branching](https://learngitbranching.js.org/) y crea tu primer repositorio en GitHub.

## Relacionados:
- [[proyectos-backend-para-principiantes]] #anterior 
- [[servicio-de-hospedaje-de-repositorios]] #siguiente 