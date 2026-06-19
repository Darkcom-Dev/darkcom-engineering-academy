# Código asistido por IA

Los **agentes de código asistido por IA** son herramientas que integran modelos de lenguaje directamente en el flujo de desarrollo. No solo autocompletan: entienden tu proyecto, ejecutan comandos, crean archivos y refactorizan.

```mermaid
flowchart TB
    subgraph Evolucion ["📈 Evolución de las herramientas"]
        E1["🔤 Autocompletado básico<br/>(Snippets, tab)"]
        E2["🤖 Copilot<br/>(sugiere líneas)"]
        E3["🧠 Agentes de código<br/>(entienden contexto,<br/>ejecutan comandos,<br/>crean archivos)"]
    end

    E1 --> E2 --> E3

    style Evolucion fill:#f5f5f5
    style E1 fill:#ffe0b2
    style E2 fill:#fff3e0
    style E3 fill:#c8e6c9
```

### ¿Qué hace un agente de código?

```mermaid
graph LR
    Dev["👨‍💻 Tú dices:<br/>'Crea una API REST<br/>de usuarios'"] --> Agent["🤖 Agente IA"]

    Agent --> Understand["📖 Entiende tu proyecto<br/>(lenguaje, frameworks,<br/>estructura de carpetas)"]
    Agent --> Plan2["📋 Planea los pasos"]
    Agent --> Execute["⚡ Ejecuta:<br/>📁 Crea archivos<br/>✏️ Escribe código<br/>📦 Instala dependencias<br/>🔧 Corre comandos"]
    Agent --> Show["👁️ Te muestra el resultado<br/>y espera tu feedback"]

    style Dev fill:#e1f5fe
    style Agent fill:#c8e6c9
    style Understand fill:#fff3e0
    style Plan2 fill:#e8eaf6
    style Execute fill:#fce4ec
    style Show fill:#e8f5e9
```

> **Diferencia clave:** Un autocompletado sugiere la siguiente línea. Un agente entiende tu proyecto y hace cambios complejos por ti.

---

## Claude Code

**Claude Code** es el agente de código de Anthropic, creado por los mismos que hicieron Claude. Funciona en la terminal y está diseñado para tareas complejas de ingeniería de software.

```mermaid
flowchart TB
    subgraph ClaudeFeatures ["🧠 Claude Code"]
        Terminal["💻 En la terminal<br/>npm @anthropic-ai/claude-code"]
        Context["📖 Contexto profundo<br/>Lee tu proyecto completo"]
        Tools["🛠️ Herramientas<br/>Lee/escribe archivos,<br/>ejecuta comandos,<br/>busca código"]
        Tasks["🎯 Tareas complejas<br/>'Implementa login con<br/>JWT y tests'"]
        Multi["📁 Multifile<br/>Crea/modifica varios<br/>archivos a la vez"]
    end

    style ClaudeFeatures fill:#d4a574,color:#fff
```

### Cómo funciona

```bash
# En la terminal de tu proyecto
claude

# Luego le dices:
# "Crea un modelo de Usuario con Prisma,
#  un endpoint POST /register y sus tests"
# Claude analiza tu proyecto y ejecuta los cambios
```

**Ideal para:** Tareas complejas que requieren entender todo el proyecto, refactorizaciones grandes, análisis de código legacy.

---

## Gemini CLI

**Gemini CLI** es el agente de código de Google, basado en el modelo Gemini 2.5. Se integra en la terminal y entiende el contexto completo de tu proyecto.

```bash
# Iniciar sesión interactiva
gemini-cli

# O pasar una tarea directa
gemini-cli "Refactoriza este componente React para usar hooks modernos"
```

### Características principales

- **Contexto completo del proyecto** — entiende estructura, dependencias y config
- **Multimodal** — puede analizar capturas de pantalla, diagramas, mockups
- **Integración con Google Cloud** — despliega directo a Cloud Run, Firestore
- **1000 requests/día gratis** — generoso tier gratuito

| Ventajas                     | Ideal para                         |
| ---------------------------- | ---------------------------------- |
| Contexto masivo (1M+ tokens) | Proyectos grandes y complejos      |
| Multimodal                   | Convertir diseños a código         |
| Integración Google Cloud     | Equipos en ecosistema Google       |

---

## OpenCode

**OpenCode** es un agente de código **open source** que corre en tu terminal, escritorio o IDE. Es el más flexible porque puedes usar **cualquier modelo** (Claude, GPT, Gemini, locales).

```mermaid
graph TB
    subgraph OpenCodeFeatures ["🔓 OpenCode"]
        Source["📦 Open Source<br/>160K+ estrellas en GitHub"]
        Models["🔌 Cualquier modelo<br/>Claude, GPT, Gemini,<br/>locales (Ollama)"]
        LSP["⚡ LSP integrado<br/>El modelo entiende<br/>tipos y errores"]
        MultiSession["🔄 Múltiples agentes<br/>en paralelo"]
        Desktop["💻 Terminal +<br/>Desktop App + IDE"]
        Share["🔗 Compartir sesiones<br/>con enlaces"]
    end

    style OpenCodeFeatures fill:#e8f5e9
```

### Cómo se usa

```bash
# En la terminal
opencode

# O modo autónomo (sin preguntar)
opencode "Agrega un endpoint GET /health a la API" --yes

# Múltiples agentes en paralelo
opencode "Crea el modelo User" & 
opencode "Crea los tests de User" &
```

| Ventajas                       | Ideal para                          |
| ------------------------------ | ----------------------------------- |
| 100% open source               | Quienes quieren control total       |
| Cualquier modelo (BYOM)        | Equipos con suscripciones existentes |
| LSP integrado                  | Proyectos donde la precisión importa |
| Multi-session paralelo         | Tareas grandes divididas            |

---

## Cursor

**Cursor** es un **editor de código** (fork de VS Code) con IA integrada en cada aspecto. No es un plugin, es un IDE diseñado desde cero para trabajar con IA.

```mermaid
flowchart TB
    subgraph CursorFeatures ["✏️ Cursor IDE"]
        Inline["✏️ Edición inline<br/>Cmd+K para editar<br/>selección con IA"]
        Chat["💬 Chat contextual<br/>Sabe qué archivo<br/>tienes abierto"]
        Composer["🎼 Composer<br/>Crea/modifica múltiples<br/>archivos a la vez"]
        Agent["🧠 Agent mode<br/>Ejecuta comandos,<br/>busca, refactoriza"]
        Terminal["💻 Terminal IA<br/>Escribe comandos<br/>por ti"]
        Docs["📖 Docs integration<br/>Importa docs de<br/>librerías externas"]
    end

    style CursorFeatures fill:#e3f2fd
```

### Cómo se usa

```bash
# Atajos clave en Cursor:
Cmd+K      # Editar código seleccionado con IA
Cmd+L      # Chat contextual (sabe qué archivo tienes abierto)
Cmd+I      # Composer (multifile)

# Agent mode:
# "Instala Tailwind, configura el archivo base,
#  y crea un componente Navbar responsivo"
```

| Ventajas                       | Ideal para                          |
| ------------------------------ | ----------------------------------- |
| IDE completo (fork VS Code)    | Quienes quieren IA en todo momento  |
| Edición inline ultra rápida    | Cambios rápidos sin cambiar contexto |
| Composer multifile             | Tareas que afectan varios archivos  |
| Agent mode                     | Automatización de tareas complejas  |

---

## Antigravity

**Antigravity** es un agente de código autónomo especializado en **tareas de ingeniería complejas y de larga duración**. A diferencia de otros asistentes que requieren supervisión constante, Antigravity puede trabajar de forma independiente durante períodos prolongados.

```mermaid
flowchart LR
    Task["📋 Tarea asignada"] --> Plan3["📝 Planifica"]
    Plan3 --> Execute2["⚡ Ejecuta"]
    Execute2 --> Verify["✅ Verifica"]
    Verify -->|"❌ Falla"| Fix["🔧 Corrige"]
    Fix --> Execute2
    Verify -->|"✅ Listo"| Done["🎉 Completado"]

    style Task fill:#e1f5fe
    style Plan3 fill:#fff3e0
    style Execute2 fill:#c8e6c9
    style Verify fill:#fce4ec
    style Fix fill:#e8eaf6
    style Done fill:#e8f5e9
```

**Ideal para:** Automatización de procesos complejos, migraciones de código a gran escala, tareas que requieren iteración y autoverificación.

---

## Comparativa rápida

```mermaid
flowchart TB
    Elegir{"🔍 ¿Qué buscas?"}

    Elegir -->|"Agente en terminal<br/>para tareas complejas"| CC["🧠 Claude Code<br/>Contexto profundo<br/>Multi-archivo"]
    Elegir -->|"Contexto masivo<br/>+ multimodal"| GC["✨ Gemini CLI<br/>1M+ tokens<br/>Imágenes + código"]
    Elegir -->|"Open source<br/>cualquier modelo"| OC["🔓 OpenCode<br/>Flexible, LSP,<br/>multi-sesión"]
    Elegir -->|"IDE con IA<br/>integrada"| CUR["✏️ Cursor<br/>Editor completo<br/>Edición inline"]
    Elegir -->|"Agente autónomo<br/>tareas largas"| AG["🔄 Antigravity<br/>Autónomo,<br/>autoverificación"]

    style Elegir fill:#e1f5fe
    style CC fill:#d4a574,color:#fff
    style GC fill:#1565c0,color:#fff
    style OC fill:#c8e6c9
    style CUR fill:#e3f2fd
    style AG fill:#f3e5f5
```

### Tabla comparativa

| Herramienta      | Tipo          | Modelos              | Open Source | Ideal para                    |
| ---------------- | ------------- | -------------------- | ----------- | ----------------------------- |
| **Claude Code**  | Terminal      | Claude               | ❌          | Tareas complejas, contexto    |
| **Gemini CLI**   | Terminal      | Gemini               | ❌          | Contexto masivo, multimodal   |
| **OpenCode**     | Terminal/IDE  | Cualquiera           | ✅          | Flexibilidad, control total   |
| **Cursor**       | IDE completo  | Claude, GPT, Gemini  | ❌          | Edición inline, productividad |
| **Antigravity**  | Agente CLI    | Múltiples            | ❌          | Tareas autónomas, migraciones |

---

## Flujo de trabajo con agentes

```mermaid
flowchart TB
    Idea["💡 Idea o tarea"] --> Prompt["✍️ Describe claramente<br/>lo que necesitas"]
    Prompt --> AgentExec["🤖 Agente ejecuta"]
    AgentExec --> Review2["👁️ Revisas el resultado"]

    Review2 -->|"✅ Aceptas"| Done2["🎉 Tarea completada"]
    Review2 -->|"❌ No es exacto"| Feedback["💬 Das feedback<br/>'cambia X por Y'"]
    Feedback --> AgentExec

    style Idea fill:#e1f5fe
    style Prompt fill:#fff3e0
    style AgentExec fill:#c8e6c9
    style Review2 fill:#fce4ec
    style Done2 fill:#e8f5e9
    style Feedback fill:#fff9c4
```

### Consejos para sacarles el máximo partido

1. **Sé específico** — "Crea un endpoint POST /register" > "Haz un registro de usuarios"
2. **Itera** — No esperes perfección al primer intento. Da feedback
3. **Revisa siempre** — La IA comete errores. Tú eres el responsable del código
4. **Divide tareas grandes** — "Implementa el modelo" → "Crea el endpoint" → "Añade tests"
5. **Usa el contexto a tu favor** — Mantén archivos relevantes abiertos para que el agente los vea

---

## Resumen visual

```mermaid
graph TB
    Title["🤖 Agentes de Código"] --> Terminal2["💻 Terminal"] & IDE["✏️ IDE"] & Auto["🔄 Autónomo"]

    Terminal2 --> Claude2["Claude Code<br/>Contexto profundo"]
    Terminal2 --> Gemini2["Gemini CLI<br/>Contexto masivo"]
    Terminal2 --> OpenCode2["OpenCode<br/>Multi-modelo"]

    IDE --> Cursor2["Cursor<br/>Edición inline"]

    Auto --> Antigravity2["Antigravity<br/>Tareas autónomas"]

    Title --> All["☝️ Todos requieren:<br/>👁️ Supervisión humana<br/>🎯 Prompts claros<br/>🔄 Iteración"]

    style Title fill:#e1f5fe
    style Terminal2 fill:#fff3e0
    style IDE fill:#e8eaf6
    style Auto fill:#f3e5f5
    style All fill:#f5f5f5
```

---

> **Siguiente paso:** Elige una herramienta según tu flujo. ¿Pasas mucho en la terminal? Prueba Claude Code u OpenCode. ¿Prefieres un IDE? Cursor. ¿Tareas masivas? Gemini CLI o Antigravity. Lo importante es **empezar a usarlas**.

## Relacionados:
- [[aplicaciones-con-ia]] #anterior 
- [[prompt-engineering]] #relacionado
- [[proveedores-de-ia]] #siguiente 