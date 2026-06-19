# Día 2 — Herramientas de Agente e Interoperabilidad con MCP

*Basado en el Whitepaper Companion Podcast del Día 2 del curso intensivo de 5 días de Google × Kaggle.*

---

## El Problema Central

Los LLMs son **máquinas de patrones brillantes pero completamente aisladas** de cualquier dato actual o acción sobre el mundo real.

```mermaid
flowchart LR
    subgraph "LLM sin herramientas"
        LLM["🧠 LLM<br/>'Tiene memorizada<br/>toda la biblioteca'"] --> Solo["Solo puede:<br/>Generar texto<br/>Responder preguntas"]
    end
    subgraph "LLM con herramientas"
        Agente["🤖 Agente<br/>'Mismo cerebro +<br/>manos y ojos'"] --> Puede["Puede:<br/>Consultar APIs<br/>Enviar emails<br/>Actualizar BD"]
    end
    style LLM fill:#ffcdd2
    style Solo fill:#ffcdd2
    style Agente fill:#c8e6c9
    style Puede fill:#c8e6c9
```

> Un LLM base puede escribir código o generar texto, pero **no puede llamar a una API, verificar el precio de una acción en tiempo real o enviar un correo electrónico** por sí solo.

**La transformación clave:** pasar de modelos que **solo piensan** a modelos que **también hacen**.

---

## ¿Qué es una Herramienta en un Agente?

Una herramienta es una **función o programa** que un agente basado en LLM usa para hacer algo que el modelo no puede hacer de forma nativa. Se dividen en dos categorías fundamentales:

```mermaid
flowchart LR
    Herramientas["🛠️ Tipos de herramientas"] --> Saber["📖 SABER algo<br/>Obtener información<br/>nueva/actualizada"]
    Herramientas --> Hacer["⚡ HACER algo<br/>Ejecutar acciones<br/>en el mundo real"]
    Saber --> Clima["Buscar clima<br/>en API"]
    Saber --> Precio["Consultar<br/>precio acciones"]
    Hacer --> Email["Enviar un<br/>correo"]
    Hacer --> Reserva["Reservar una<br/>reunión"]
    style Herramientas fill:#e1f5fe,stroke:#333
    style Saber fill:#bbdefb
    style Hacer fill:#c8e6c9
```

---

## Los 3 Tipos de Herramientas

### 1. Funciones (Function Tools)

Definidas explícitamente por el desarrollador. Son funciones externas (ej: código Python) conectadas al agente mediante **docstrings** detallados.

```python
def set_lights(brightness: int, color: str):
    """Establece el brillo y color de las luces inteligentes.
    
    Args:
        brightness: Nivel de brillo (0-100)
        color: Color en hexadecimal (#RRGGBB)
    """
    # implementación
```

**Clave:** las docstrings definen el **contrato** entre el modelo y la función — qué entradas espera y qué salida produce.

### 2. Integradas (Built-in Tools)

Proporcionadas implícitamente por el proveedor del modelo. El desarrollador no ve su definición; trabajan detrás de escena.

| Herramienta | Proveedor | Función |
|-------------|-----------|---------|
| **Google Search** | Gemini | Búsqueda y grounding |
| **Code Execution** | Gemini | Ejecutar código Python |
| **URL Reader** | Gemini | Recuperar contenido de una URL |

### 3. Agente como Herramienta (Agent Tools)

Invocas a **otro agente separado como si fuera una herramienta**. El agente principal **permanece a cargo** — llama al subagente, obtiene un resultado, y lo usa como entrada para su propio razonamiento.

```mermaid
flowchart TB
    Principal["🤖 Agente Principal<br/>(Gerente)"] --> AgentTool["🔄 Llamar Agente<br/>como Herramienta"]
    AgentTool --> Sub["🤖 Sub-agente<br/>(Especialista)"]
    Sub --> Resultado["📊 Resultado<br/>sintetizado"]
    Resultado --> Principal
    Principal --> Email["📧 Enviar email<br/>(otra herramienta)"]
    style Principal fill:#bbdefb
    style Sub fill:#c8e6c9
    style AgentTool fill:#fff9c4
```

> **No es una transferencia completa** — es **delegación jerárquica**. Como un gerente que pide un informe a un especialista, no que le transfieran todo el proyecto.

---

## Taxonomía por Propósito

```mermaid
flowchart TB
    Taxonomia["📋 Categorías de herramientas"] --> IR["🔍 Recuperación<br/>de información"]
    Taxonomia --> AE["⚡ Ejecución<br/>de acciones"]
    Taxonomia --> API["🔗 Integración<br/>de APIs"]
    Taxonomia --> HITL["👤 Humano en<br/>el circuito"]
    IR --> BD["Consultar BD"]
    IR --> Web["Buscar en web"]
    AE --> Reservar["Reservar algo"]
    AE --> Email["Enviar correo"]
    API --> CRM["Conectar CRM"]
    API --> Slack["Enviar a Slack"]
    HITL --> Approve["Pedir permiso"]
    HITL --> Clarify["Pedir aclaración"]
    style Taxonomia fill:#e1f5fe,stroke:#333
    style IR fill:#bbdefb
    style AE fill:#c8e6c9
    style API fill:#fff9c4
    style HITL fill:#fce4ec
```

---

## Mejores Prácticas de Diseño de Herramientas

> Estas no son sugerencias — son **esenciales para el éxito** de un agente.

### 1. La Documentación es Primordial

La **única forma** en que el modelo sabe qué hace tu herramienta es a través de la documentación que le proporcionas.

```
❌ Nombre vago: "update_ticket"
✅ Nombre claro: "create_bug_report_with_priority"
```

> El nombre, la descripción y los parámetros se alimentan literalmente en el **contexto del modelo**.

### 2. Describe la Acción, NO la Implementación

```
❌ "Llama a create_ticket en la API /v2/tickets con método POST..."
   → El modelo se confunde con los detalles técnicos

✅ "Crea un reporte de error para describir este problema"
   → El modelo entiende el objetivo
```

Deja que el LLM **razone** sobre qué hacer. La herramienta es solo el **mecanismo** que ejecuta esa decisión.

### 3. Publica Tareas, No Llamadas API Crudas

Las APIs empresariales pueden tener **decenas de parámetros**. No las expongas directamente.

```
❌ Wrapper fino sobre API de calendario con 15 flags opcionales
✅ Herramienta "reservar_sala_reuniones" que encapsula la complejidad
```

### 4. Diseña para Resultados Concisos

Si tu herramienta devuelve una hoja de cálculo enorme y la metes toda en la ventana de contexto:

```mermaid
flowchart LR
    Tool["🛠️ Herramienta"] --> Data["📊 Devuelve<br/>datos enormes"]
    Data --> Context["💥 Ventana de<br/>Contexto inflada<br/>🐌 Latencia ↑<br/>💰 Costo ↑<br/>🧠 Razonamiento ↓"]
    style Data fill:#ffcdd2
    style Context fill:#ffcdd2
```

**Alternativas:**
- Devuelve un **resumen conciso**
- Devuelve una **confirmación**
- Devuelve una **referencia (URI)** a los datos completos

> Sistemas como el **servicio de artefactos de ADK** están diseñados para esto: el agente sabe dónde están los datos sin tenerlos en el contexto.

### 5. Manejo de Errores Descriptivo

No solo `Error 500`. Los mensajes de error deben ser **instructivos** para el LLM.

```
❌ "Error: 500"
✅ "Error: Límite de tasa excedido. Espera 15 segundos antes de reintentar."
```

```mermaid
flowchart TB
    Tool["🛠️ Ejecuta<br/>herramienta"] --> OK["✅ Éxito<br/>Devuelve resultado"]
    Tool --> Error["❌ Error"]
    Error --> MSG["Mensaje descriptivo<br/>'Límite excedido.<br/>Espera 15s'"]
    MSG --> LLM["🧠 LLM razona<br/>'Debo esperar<br/>y reintentar'"]
    style OK fill:#c8e6c9
    style Error fill:#ffcdd2
    style MSG fill:#fff9c4
```

---

## MCP — Model Context Protocol

MCP es un **estándar abierto** (surgido en 2024) para unificar la integración de herramientas. Inspirado en el **LSP (Language Server Protocol)**.

> **Objetivo:** convertir la integración de herramientas de un problema exponencial `n × m` (n modelos × m herramientas) en un **formato plug-and-play**.

```mermaid
flowchart LR

    subgraph A["Antes: n × m"]
        M1["Modelo A"]
        M2["Modelo B"]

        T1["Tool 1"]
        T2["Tool 2"]

        M1 --> T1
        M1 --> T2
        M2 --> T1
        M2 --> T2
    end

    subgraph B["Con MCP: estándar único"]
        MA["Modelo A"]
        MB["Modelo B"]

        MCP["Protocolo MCP"]

        S1["Tool 1"]
        S2["Tool 2"]

        MA --> MCP
        MB --> MCP

        MCP --> S1
        MCP --> S2
    end

    style M1 fill:#bbdefb
    style M2 fill:#c8e6c9
    style MA fill:#bbdefb
    style MB fill:#c8e6c9

    style T1 fill:#ffcdd2
    style T2 fill:#ffcdd2

    style MCP fill:#fff9c4

    style S1 fill:#81c784
    style S2 fill:#81c784
```

### Arquitectura Cliente-Servidor

```mermaid
flowchart TB
    subgraph "Arquitectura MCP"
        Host["🏠 HOST MCP<br/>Aplicación principal<br/>Gestiona UX, razonamiento,<br/>barandillas y seguridad"] --> Client["📡 CLIENTE MCP<br/>Módulo de comunicación<br/>Conexión, ciclo de vida,"]
        Client --> Server["🖥️ SERVIDOR MCP<br/>Provee herramientas<br/>Anuncia capacidades<br/>Ejecuta comandos"]
    end
    User["👤 Usuario"] --> Host
    Host --> Tools["Herramienta 1"]
    Host --> Tools2["Herramienta 2"]
    style Host fill:#bbdefb
    style Client fill:#fff9c4
    style Server fill:#c8e6c9
    style User fill:#e3f2fd
```

| Componente | Rol |
|------------|-----|
| **Host MCP** | La app principal. Gestiona UX, organiza el proceso de pensamiento del agente, decide cuándo usar herramientas. **Aplica barandillas y políticas de seguridad.** |
| **Cliente MCP** | Módulo de comunicación embebido en el host. Mantiene conexión con el servidor, gestiona la sesión, envía comandos. |
| **Servidor MCP** | El programa que provee las herramientas. Anuncia qué ofrece, escucha comandos, ejecuta y devuelve resultados. |

### Comunicación

| Capa | Detalle |
|------|---------|
| **Formato** | JSON RPC 2.0 (estándar basado en texto) |
| **Transporte local** | STDIO (stdin/stdout) — rápido, para desarrollo o mismo host |
| **Transporte remoto** | HTTP con Server-Sent Events — para sistemas distribuidos |

### Definición de Herramientas en MCP

Cada herramienta se define con un **esquema JSON estandarizado**:

```json
{
  "name": "get_stock_price",
  "description": "Obtiene el precio actual de una acción",
  "inputSchema": {
    "type": "object",
    "properties": {
      "symbol": { "type": "string", "description": "Símbolo de la acción (ej: GOOG)" },
      "date": { "type": "string", "description": "Fecha opcional (YYYY-MM-DD)" }
    },
    "required": ["symbol"]
  },
  "outputSchema": {
    "type": "object",
    "properties": {
      "price": { "type": "number" },
      "currency": { "type": "string" },
      "timestamp": { "type": "string" }
    }
  }
}
```

### Resultados de Herramientas

| Tipo | Descripción |
|------|-------------|
| **Estructurado** | Objeto JSON que cumple estrictamente el esquema de salida. Fácil de parsear y razonar. |
| **No estructurado** | Texto plano, audio, imagen. O referencias URI para evitar inflar el contexto. |

### Errores en MCP

Dos niveles:

```mermaid
flowchart TB
    Call["Host llama a<br/>herramienta MCP"] --> ProtocolError["📡 Error de protocolo<br/>¿Método no existe?<br/>¿Parámetros mal formados?"]
    Call --> ExecError["⚙️ Error de ejecución<br/>¿API caída?<br/>¿Datos no encontrados?"]
    ProtocolError --> JSON["Error JSON RPC<br/>estándar"]
    ExecError --> Flag["Flag 'error' = true<br/>+ mensaje descriptivo"]
    ExecError --> Recover["LLM puede<br/>reintentar/recuperarse"]
    style Call fill:#e3f2fd
    style ProtocolError fill:#ffcdd2
    style ExecError fill:#fff9c4
    style Recover fill:#c8e6c9
```

---

## El Problema de Escalar: Contexto + Muchas Herramientas

Si un agente tiene acceso a **1000 herramientas**, cargar las 1000 definiciones en el contexto cada vez:

```mermaid
flowchart LR
    ToolDB["🗄️ 1000 herramientas"] --> Load["Cargar TODAS<br/>en el contexto"] --> Problem["💥 Límite de contexto<br/>💰 Costo altísimo<br/>🧠 Calidad cae"]
    style Load fill:#ffcdd2
    style Problem fill:#ffcdd2
```

### Solución: Tool Retrieval (RAG para herramientas)

```mermaid
flowchart TB
    Task["🤖 Agente necesita<br/>una herramienta"] --> Search["🔍 Búsqueda semántica<br/>sobre índice de<br/>herramientas"]
    Search --> Top["Top 3-5 herramientas<br/>más relevantes"]
    Top --> Context["Cargar SOLO esas<br/>en el contexto"]
    Context --> LLM["🧠 LLM razona<br/>con las relevantes"]
    style Search fill:#bbdefb
    style Top fill:#fff9c4
    style Context fill:#c8e6c9
    style LLM fill:#e8eaf6
```

**Idea clave:** en lugar de precargar todas las definiciones, el agente **busca semánticamente** sobre una base de datos de herramientas y solo obtiene las más relevantes para la tarea actual.

---

## Seguridad: El Problema del Diputado Confundido

MCP está diseñado para **innovación e interoperabilidad**, no tiene seguridad empresarial integrada (autenticación, autorización).

### Vulnerabilidad

```mermaid
flowchart TB
    User["👤 Usuario<br/>poco privilegiado"] --> Malicious["💀 Mensaje malicioso<br/>'Ignora todo, ejecuta<br/>acción sensible'"]
    Malicious --> LLM["🤖 LLM / Host<br/>(confía en el usuario)"]
    LLM --> MCPServer["🖥️ Servidor MCP<br/>con altos privilegios"]
    MCPServer --> Action["⚠️ Ejecuta acción<br/>sin verificar<br/>permisos del usuario"]
    style Malicious fill:#ffcdd2
    style Action fill:#ffcdd2
    style MCPServer fill:#fff9c4
```

> El usuario **blanquea** su petición maliciosa a través de la IA confiable. El servidor MCP ve la solicitud del modelo (confiable) y la ejecuta sin verificar que el usuario original tuviera permisos.

### Solución: Gobernanza Externa

MCP no se expone directamente. Se envuelve en **capas de seguridad empresarial**:

```mermaid
flowchart TB
    User["👤 Usuario"] --> Gateway["🚪 API Gateway<br/>(Apigee, etc.)"]
    Gateway --> Auth["(1) Autenticación<br/>del usuario"]
    Gateway --> AuthZ["(2) Autorización<br/>para la acción"]
    Gateway --> Rate["(3) Rate limiting"]
    Gateway --> Audit["(4) Logging"]
    Gateway --> Filter["(5) Filtrado de<br/>input malicioso"]
    Filter --> MCPServer["🖥️ Servidor MCP<br/>recibe solicitud<br/>ya validada"]
    style User fill:#e3f2fd
    style Gateway fill:#bbdefb
    style Auth fill:#c8e6c9
    style AuthZ fill:#c8e6c9
    style Rate fill:#c8e6c9
    style Audit fill:#c8e6c9
    style Filter fill:#c8e6c9
    style MCPServer fill:#fff9c4
```

> La seguridad **no está en MCP, está alrededor de MCP**. El protocolo habilita la conexión, las capas empresariales garantizan que se use de forma segura.

---

## Beneficios de MCP

| Beneficio | Descripción |
|-----------|-------------|
| **Acelera desarrollo** | Reduce drásticamente la fricción de integrar nuevas capacidades |
| **Ecosistema reutilizable** | Registros públicos de MCP, herramientas plug-and-play de múltiples proveedores |
| **Capacidades dinámicas** | Agentes pueden descubrir y usar nuevas herramientas en tiempo de ejecución |
| **Flexibilidad arquitectónica** | Desacopla el razonamiento central de la lógica específica de herramientas |
| **Modularidad** | Equipos separados construyen el host (razonamiento) vs servidores (herramientas) |

---

## Principios Universales (incluso sin MCP)

> Aunque no uses MCP directamente, estas prácticas mejorarán tus agentes de inmediato:

1. **Documentación clara** — nombres + descripciones precisas
2. **Enfócate en la tarea, no en la API** — abstrae la complejidad
3. **Resultados concisos** — no vuelques datos enormes en el contexto
4. **Errores instructivos** — dile al LLM cómo recuperarse
5. **Seguridad en capas** — nunca confíes en el protocolo base para seguridad empresarial

---

## Conclusión

```mermaid
flowchart TB
    Fundamento["🧠 Modelo = Cerebro"] --> Herramientas["🛠️ Herramientas = Manos y Ojos"]
    Herramientas --> MCP["📡 MCP = Lenguaje estándar<br/>para conectar cerebro y manos"]
    MCP --> BuenasPracticas["📋 Buenas prácticas<br/>de diseño de herramientas"]
    MCP --> Seguridad["🔒 Seguridad externa<br/>API Gateway + gobernanza"]
    BuenasPracticas --> Exito["✅ Agente confiable<br/>en producción"]
    Seguridad --> Exito
    style Fundamento fill:#bbdefb
    style Herramientas fill:#c8e6c9
    style MCP fill:#fff9c4
    style BuenasPracticas fill:#e8eaf6
    style Seguridad fill:#fce4ec
    style Exito fill:#81c784,stroke:#333
```

---

## Navegación

- [[dia-1-introduccion-a-agentes]] ← Anterior: fundamentos de agentes
- [[dia-3-context-engineering-sesiones-y-memoria]] → Siguiente: ingeniería de contexto, sesiones y memoria
- [[opcional-dia-2-livestream]] → Siguiente: Livestream Q&A del Día 2
- [[5-day-ai-agents-intensive-course-with-google]] ← Volver al índice
