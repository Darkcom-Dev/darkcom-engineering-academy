# Día 5 — De Prototipo a Producción

*Basado en el Whitepaper Companion Podcast del Día 5 del curso intensivo de 5 días de Google × Kaggle.*

---

## El Problema Central: La Brecha de la Última Milla

> **El 80% del esfuerzo no es la inteligencia central de la IA — es la infraestructura, la seguridad, la validación y la plomería necesaria para hacer que un agente funcione de forma segura y confiable en producción.**

```mermaid
flowchart LR
    subgraph Prototype["🧪 Prototipo (Minutos)"]
        P1["Demo impresionante<br/>Agente funcional<br/>en local"]
    end

    subgraph Gap["⚡ Brecha del 80%"]
        G1["Infraestructura"]
        G2["Seguridad"]
        G3["Validación"]
        G4["CI/CD"]
        G5["Observabilidad"]
        G6["Gobernanza"]
    end

    subgraph Production["🚀 Producción"]
        Pr1["Escalable, seguro<br/>confiable, monitoreado"]
    end

    Prototype --> Gap --> Production

    style Prototype fill:#c8e6c9
    style Gap fill:#ffcdd2
    style Production fill:#bbdefb
```

---

## Las 3 Ideas Centrales

```mermaid
flowchart TB
    Title["🏗️ Prototipo → Producción"] --> Eval["📐 Evaluación<br/>Automatizada"]
    Title --> Deploy["🚀 Despliegue<br/>Automatizado"]
    Title --> Obs["👁️ Observabilidad<br/>Integral"]

    Eval --> E1["Pipeline CI/CD con<br/>evaluación como puerta<br/>de calidad obligatoria"]
    Deploy --> D1["Canary, Blue-Green,<br/>A/B Testing con<br/>retroceso instantáneo"]
    Obs --> O1["Logs + Trazas + Métricas<br/>→ Bucle Operativo<br/>Observar-Actuar-Evolucionar"]

    style Title fill:#e1f5fe,stroke:#333
    style Eval fill:#c8e6c9
    style Deploy fill:#bbdefb
    style Obs fill:#fff9c4
```

---

## Personas y Procesos: El Paso Cero

Antes de la tecnología, define los roles:

```mermaid
flowchart TB
    subgraph Team["👥 Equipo de Operaciones de Agentes"]
        PE["🎯 Ingeniero de Prompts<br/>→ Define la 'constitución' del agente<br/>→ Límites de seguridad<br/>→ Criterios de respuesta buena/mala"]
        AIE["⚙️ Ingeniero de IA<br/>→ Construye sistemas backend robustos<br/>→ Integra barandillas<br/>→ Implementa CI/CD y evaluación"]
        Cloud["☁️ Ingeniero de Plataforma<br/>→ Autenticación y autorización<br/>→ Infraestructura escalable"]
    end

    Team --> Governance["📋 Gobernanza:<br/>Coordinación estrecha entre roles"]

    style Team fill:#e3f2fd
    style Governance fill:#fff9c4
```

> **❌ Error común:** El equipo de cloud configura autenticación, pero el prompt engineer no lo sabe → el agente puede acceder a datos prohibidos. La coordinación no es opcional.

---

## Pipeline de Evaluación Cerrada (CI/CD para Agentes)

> **Ningún agente llega a producción sin pasar controles rigurosos que demuestren su eficacia y seguridad.**

### El Embudo Progresivo (Shift Left)

```mermaid
flowchart TB
    subgraph Phase1["Fase 1: Pre-Merge (CI)"]
        P1_1["Pruebas unitarias"]
        P1_2["Linting y estilo"]
        P1_3["Evaluación central<br/>del agente"]
        P1_Note["⚡ Rápido (< 5 min)<br/>Feedback inmediato"]
    end

    subgraph Phase2["Fase 2: Post-Merge & Staging (CD)"]
        P2_1["Pruebas de carga"]
        P2_2["Integración con<br/>servicios reales"]
        P2_3["Dogfooding interno"]
        P2_Note["🔍 Profundo<br/>Entorno espejo de prod"]
    end

    subgraph Phase3["Fase 3: Producción Cerrada"]
        P3_1["Canary 1% usuarios"]
        P3_2["Monitoreo continuo"]
        P3_3["Rollback automático<br/>si métricas degradan"]
        P3_Note["🚀 Gradual<br/>Mismo artefacto que staging"]
    end

    Phase1 -->|Pasa| Phase2 -->|Pasa| Phase3

    style Phase1 fill:#c8e6c9
    style Phase2 fill:#fff9c4
    style Phase3 fill:#ffcdd2
```

### Puertas de Calidad

| Tipo | Cómo Funciona |
|------|---------------|
| 🚪 **Puerta Manual** | Ingeniero ejecuta suite de evaluación → vincula reporte al PR → revisión humana obligatoria |
| 🤖 **Puerta Automática** | Pipeline CI/CD ejecuta evaluación automáticamente; si métricas caen bajo umbral → despliegue bloqueado |

---

## Estrategias de Despliegue Seguro

```mermaid
flowchart LR
    subgraph Canary["🐤 Canary"]
        C1["1% usuarios<br/>→ nueva versión"]
        C2["99% usuarios<br/>→ versión estable"]
        C3["✅ Si funciona bien<br/>→ aumentar % gradualmente"]
        C4["❌ Si falla<br/>→ retroceso instantáneo"]
    end

    subgraph BlueGreen["🔵🟢 Blue-Green"]
        BG1["Dos entornos<br/>de producción idénticos"]
        BG2["Desplegar en 🟢<br/>mientras 🔵 sirve tráfico"]
        BG3["✅ Cambiar tráfico a 🟢<br/>Cero downtime"]
        BG4["❌ Problema →<br/>volver a 🔵 al instante"]
    end

    subgraph AB["A/B Testing"]
        AB1["Comparar versiones<br/>del agente en vivo"]
        AB2["Métricas reales<br/>de negocio"]
        AB3["Gana la versión<br/>con mejor rendimiento"]
    end

    style Canary fill:#fff9c4
    style BlueGreen fill:#e3f2fd
    style AB fill:#e8f5e9
```

### Versiona Todo

> **Código, prompts, esquemas de herramientas, estructura de memoria — todo necesita un número de versión. Es tu botón de deshacer en producción.**

---

## Seguridad: Marco SIF (3 Capas)

```mermaid
flowchart TB
    Layer1["Capa 1: Definición de Políticas<br/>📜 La 'Constitución' del Agente"]
    Layer2["Capa 2: Barandillas y Filtrado<br/>🛑 Paradas bruscas y controles"]
    Layer3["Capa 3: Garantía Continua<br/>🔄 Evaluación rigurosa permanente"]

    Layer1 --> L1D["Instrucciones del sistema<br/>que definen reglas fundamentales"]
    Layer2 --> L2D["Filtrado de entrada (ej: Perspective API)<br/>Filtrado de salida (ej: PII detection)<br/>Humano en el Bucle (HITL)"]
    Layer3 --> L3D["Equipo rojo proactivo<br/>Pruebas de seguridad continuas<br/>Evaluación de IA responsable (RAI)"]

    style Layer1 fill:#c8e6c9
    style Layer2 fill:#fff9c4
    style Layer3 fill:#ffcdd2
```

### Libro de Jugadas de Respuesta a Incidentes

```mermaid
flowchart LR
    Incident["🚨 Incidente"] --> Contain["(1) Contención<br/>Disyuntor / Feature Flag<br/>Desactivar herramienta<br/>comprometida al instante"]
    Contain --> Triage["(2) Clasificación<br/>Enrutar a revisión humana (HITL)<br/>Entender alcance del ataque"]
    Triage --> Patch["(3) Resolución<br/>Desarrollar parche<br/>→ Actualizar prompt<br/>→ Fortalecer filtro<br/>→ Corregir código"]
    Patch --> Deploy["(4) Despliegue<br/>Pipeline CI/CD automatizado<br/>→ Implementar inmediatamente"]

    style Incident fill:#ffcdd2
    style Contain fill:#ffe0b2
    style Triage fill:#fff9c4
    style Patch fill:#c8e6c9
    style Deploy fill:#bbdefb
```

---

## Bucle Operativo: Observar → Actuar → Evolucionar

```mermaid
flowchart TB
    Observe["👁️ OBSERVAR<br/>Logs + Trazas + Métricas"]
    Act["🎮 ACTUAR<br/>Control operativo<br/>en tiempo real"]
    Evolve["🧬 EVOLUCIONAR<br/>Mejora continua"]

    Observe -->|"Detectar anomalías"| Act
    Act -->|"Incidente → retroalimentación"| Evolve
    Evolve -->|"Nuevo caso de prueba<br/>al golden set"| Observe

    Act --> A1["Desacoplar estado<br/>de la lógica del agente<br/>(escalado horizontal)"]
    Act --> A2["Reintentos automáticos<br/>con backoff exponencial<br/>(herramientas idempotentes)"]
    Act --> A3["Caching y límites<br/>de presupuesto<br/>para control de costos"]
    Evolve --> E1["Errores de producción<br/>→ nuevos casos de prueba<br/>en el conjunto dorado"]
    Evolve --> E2["Mejoras implementadas<br/>en horas/días,<br/>no semanas/meses"]

    style Observe fill:#bbdefb
    style Act fill:#fff9c4
    style Evolve fill:#c8e6c9
```

### Principios Arquitectónicos Clave

| Principio | Descripción |
|-----------|-------------|
| 🏗️ **Desacoplar estado de lógica** | Memoria y sesiones en base de datos externa (Cloud SQL, etc.) → escalado horizontal |
| 🔄 **Idempotencia** | Las herramientas deben poder reintentarse sin efectos secundarios (GET ok, POST de pago NO) |
| ⚡ **Caching** | Reduce latencia y costo en consultas repetitivas |
| 📊 **Muestreo dinámico** | 100% de trazas en fallos, ~10% en éxitos |

---

## Interoperabilidad: MCP vs A2A

> **Analogía del taller de reparación de coches:**
> - El **gerente** (agente orquestador) delega el diagnóstico del motor a un **mecánico** (sub-agente) usando **A2A**
> - El **mecánico** usa **MCP** para consultar el escáner de diagnóstico y la base de datos de piezas

```mermaid
flowchart TB
    subgraph A2A["🔗 A2A — Agent-to-Agent"]
        A2A_Title["Agentes colaborando<br/>para objetivos complejos"]
        A2A_Features["✅ Con estado<br/>✅ Orientado a objetivos<br/>✅ Descubrimiento vía<br/>tarjetas de agente (JSON)"]
    end

    subgraph MCP["🔌 MCP — Model Context Protocol"]
        MCP_Title["Agentes usando herramientas<br/>y recursos estáticos"]
        MCP_Features["✅ Generalmente apátrida<br/>✅ Consultas específicas<br/>✅ Interacción tipo API"]
    end

    MCP --> A2A
    MCP_Example["Ej: 'Tráeme el clima<br/>actual para Londres'"]
    A2A_Example["Ej: 'Analiza los datos<br/>de churn y sugiere<br/>3 estrategias'"]

    style A2A fill:#e3f2fd
    style MCP fill:#c8e6c9
```

### Registros de Agentes y Herramientas

Cuando tienes cientos de agentes o miles de herramientas, el **descubrimiento** se convierte en un cuello de botella:

| Registro | Propósito | Estándar |
|----------|-----------|----------|
| 🧰 **Registro de Herramientas** | Catalogar y gobernar herramientas disponibles | MCP |
| 🤖 **Registro de Agentes** | Catalogar agentes especializados con sus capacidades | A2A (tarjetas de agente) |

---

## 👀 Mirando al Futuro

### Protocolo de Pagos de Agentes (A2P)

> Así como la web floreció cuando se convirtió en un vehículo para el comercio, los agentes florecerán cuando puedan realizar transacciones de valor de forma autónoma.

```mermaid
flowchart LR
    Web["🌐 Web 1.0<br/>Información"] --> Web2["🌐 Web 2.0<br/>Comercio<br/>(HTTP + SSL + Pagos)"]
    Agents["🤖 Agentes 1.0<br/>Tareas simples"] --> Agents2["🤖 Agentes 2.0<br/>Valor<br/>(A2A + A2P)"]
```

---

## Resumen: Principios Clave

1. **El 80% del esfuerzo está en la plomería** — infraestructura, seguridad, validación
2. **Evalúa antes de desplegar** — pipeline CI/CD con puertas de calidad automáticas
3. **Despliega gradualmente** — canary, blue-green, con retroceso instantáneo
4. **Observabilidad integral** — logs, trazas, métricas para operar y mejorar
5. **A2A + MCP son complementarios** — uno para colaboración entre agentes, otro para uso de herramientas
6. **La seguridad es 3 capas** — política + barandillas + garantía continua
7. **La velocidad de evolución es la meta** — mejoras en horas/días, no semanas/meses

---

## Navegación

- [[opcional-dia-4-livestream]] ← Anterior: Livestream Q&A del Día 4
- [[opcional-dia-5-livestream]] → Siguiente: Livestream Q&A del Día 5
- [[5-day-ai-agents-intensive-course-with-google]] ← Volver al índice
