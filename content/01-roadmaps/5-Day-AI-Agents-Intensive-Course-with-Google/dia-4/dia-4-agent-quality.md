# Día 4 — Calidad del Agente

*Basado en el Whitepaper Companion Podcast del Día 4 del curso intensivo de 5 días de Google × Kaggle.*

---

## El Problema Central: No Determinismo

> **Los agentes de IA son inherentemente no deterministas.** No puedes predecir exactamente qué ruta tomará un agente para resolver una tarea. El éxito no es binario — los agentes fallan de forma insidiosa, no espectacular.

```mermaid
flowchart LR
    Input["📝 Entrada del Usuario"] --> Agent["🤖 Agente<br/>Toma decisiones<br/>en tiempo real"]
    Agent --> Path1["Ruta A<br/>3 pasos, eficiente"]
    Agent --> Path2["Ruta B<br/>10 pasos, correcta"]
    Agent --> Path3["Ruta C<br/>25 pasos con errores<br/>pero resultado correcto"]
    Path1 --> Success["✅ Éxito"]
    Path2 --> Success
    Path3 --> Hidden["⚠️ Éxito aparente<br/>pero trayectoria<br/>problemática"]
    style Path1 fill:#c8e6c9
    style Path2 fill:#fff9c4
    style Path3 fill:#ffcdd2
    style Hidden fill:#ffe0b2
```

**El problema clave:** Un agente puede obtener la respuesta correcta por las razones equivocadas. **La trayectoria importa tanto como el resultado.**

---

## Las 3 Ideas Centrales

```mermaid
flowchart TB
    Title["🏗️ Calidad del Agente"] --> Traj["🔍 La Trayectoria<br/>es la Verdad"]
    Title --> Obs["👁️ La Observabilidad<br/>es la Base"]
    Title --> Loop["🔄 La Evaluación<br/>es un Bucle Continuo"]

    Traj --> Detail1["No solo el resultado final<br/>importa — todo el proceso<br/>de toma de decisiones cuenta"]
    Obs --> Detail2["Registros, trazas y métricas<br/>son los 3 pilares para<br/>ver dentro del agente"]
    Loop --> Detail3["El Volante de Calidad:<br/>cada interacción en producción<br/>retroalimenta la mejora"]

    style Title fill:#e1f5fe,stroke:#333
    style Traj fill:#bbdefb
    style Obs fill:#c8e6c9
    style Loop fill:#fff9c4
```

---

## Analogía: Camión de Reparto vs. Coche de F1

| Aspecto | 🚚 Software Tradicional | 🏎️ Agente de IA |
|---------|------------------------|-----------------|
| **Comportamiento** | Ruta fija, tareas predecibles | Decisiones dinámicas en tiempo real |
| **Fallo** | Explícito (crash, error 500) | Insidioso (reporta 200 OK pero da resultado incorrecto) |
| **Prueba** | Checklist binario (pasa/falla) | Validación continua de trayectoria |
| **Ejemplo** | Calculadora | Agente que "casi" comete un error pero se autocorrige |

### Modos de Fallo Específicos de Agentes

| Modo | Descripción | Ejemplo |
|------|-------------|---------|
| 🎯 **Sesgo Algorítmico** | Amplifica sesgos de datos de entrenamiento | Agente de selección de CVs penaliza candidatos por género |
| 🧠 **Alucinación** | Inventa fuentes, datos o razonamientos | Agente cita un artículo falso como evidencia |
| 📉 **Deriva Conceptual** | El mundo cambia pero el agente no se actualiza | Sistema de detección de fraude entrenado en estafas del año pasado |
| 👻 **Emergentes No Deseados** | Desarrolla "supersticiones" o explota lagunas | Agente encuentra un atajo no previsto en las reglas |

---

## Los 4 Pilares de la Calidad

```mermaid
flowchart TB
    Pillars["🏛️ Pilares de la Calidad del Agente"] --> Efect["🎯 Efectividad"]
    Pillars --> Efic["⚡ Eficiencia"]
    Pillars --> Robu["🛡️ Robustez"]
    Pillars --> Safe["🔒 Seguridad y<br/>Alineación"]

    Efect --> E1["¿El agente logró lo<br/>que el usuario pretendía?"]
    Efect --> E2["No solo completar la tarea,<br/>sino satisfacer la necesidad"]
    Efic --> F1["Latencia, costo (tokens),<br/>complejidad de la ruta"]
    Efic --> F2["¿Solución directa de 3 pasos<br/>o serpenteo de 25?"]
    Robu --> R1["¿Maneja errores de API,<br/>problemas de red,<br/>instrucciones ambiguas?"]
    Robu --> R2["¿Reintenta con gracia o<br/>simplemente falla?"]
    Safe --> S1["Ética, evitar daños,<br/>resistir prompt injection"]
    Safe --> S2["Si este pilar falla,<br/>los demás no importan"]

    style Pillars fill:#e1f5fe,stroke:#333
    style Efect fill:#bbdefb
    style Efic fill:#c8e6c9
    style Robu fill:#fff9c4
    style Safe fill:#ffcdd2
```

---

## Jerarquía de Evaluación: De Afuera Hacia Adentro

```mermaid
flowchart TB
    subgraph BlackBox["📦 Caja Negra (Exterior)"]
        E2E["Evaluación Extremo a Extremo"]
        E2E_Result["¿El agente completó la tarea?<br/>Puntuación CSAT, tasa de éxito"]
    end

    subgraph GlassBox["🔍 Caja de Cristal (Interior)"]
        Trajectory["Evaluación de Trayectoria"]
        Traj1["¿El plan era correcto?"]
        Traj2["¿Usó las herramientas adecuadas?"]
        Traj3["¿Interpretó bien las respuestas?"]
        Traj4["¿La cadena de pensamiento es sólida?"]
    end

    BlackBox -->|"Si falla → diagnosticar"| GlassBox
    GlassBox -->|"Hallazgos → mejorar"| BlackBox

    style BlackBox fill:#e3f2fd
    style GlassBox fill:#fff3e0
```

**1️⃣ Caja Negra:** Mira solo el resultado final. ¿Éxito o fracaso? No dice *por qué*.

**2️⃣ Caja de Cristal:** Abre la trayectoria. Analiza el plan, uso de herramientas, interpretación de respuestas, cadena de pensamiento.

> 💡 **Consejo práctico con ADK:** Guarda una ejecución exitosa como `test.json` — captura toda la secuencia de llamadas a herramientas y pensamientos. Se convierte en tu **verdad fundamental** para pruebas de regresión.

---

## El Sistema Híbrido de Evaluación

```mermaid
flowchart LR
    subgraph Auto["🤖 Automatizado"]
        M1["Métricas Básicas<br/>ROUGE, BERTScore"]
        M2["LLM-as-Judge<br/>Comparación por pares<br/>con rúbrica"]
    end

    subgraph Human["👤 Humano en el Bucle"]
        H1["Revisión con UI<br/>que muestra historial<br/>+ trayectoria lado a lado"]
        H2["Aprobación para<br/>acciones críticas<br/>(pagos, emails sensibles)"]
    end

    Auto --> Human
    Human -->|"Feedback → calibrar"| Auto

    style Auto fill:#e8f5e9
    style Human fill:#fff3e0
```

### LLM-as-Judge: Cómo Hacerlo Bien

| Mala Práctica ❌ | Buena Práctica ✅ |
|-----------------|-------------------|
| "Puntúa esta respuesta del 1 al 5" → sesgo de tendencia central (todos sacan 3) | Comparación por pares: "¿Salida A o B es mejor para esta tarea?" |
| Sin rúbrica → el juez aplica sus propios criterios inconsistentes | Rúbrica estructurada con dimensiones específicas (seguridad, utilidad, tono) |
| Un solo modelo juzga → sesgo de autopreferencia | Múltiples modelos como jueces, medir acuerdo entre ellos |
| Pregunta vaga "¿es buena esta respuesta?" | "Razona paso a paso contra estos criterios predefinidos" |

### Técnicas Avanzadas

- **Agente como Juez:** Entrena un agente especializado cuyo trabajo es evaluar la calidad del razonamiento y las opciones de herramientas de otro agente
- **Autoconsistencia:** Ejecuta el mismo juez múltiples veces sobre una trayectoria novedosa — si hay alta variación, el juez no es confiable
- **Clustering de Trayectorias:** Usa agrupación no supervisada para visualizar el espacio de trayectorias y detectar novedades

---

## Observabilidad: Los 3 Pilares

> **El seguimiento (monitoring) verifica si un cocinero siguió la receta. La observabilidad permite entender cómo un chef gourmet improvisa en un desafío de ingredientes sorpresa.**

```mermaid
flowchart TB
    Obs["👁️ Observabilidad"] --> Logs["📋 Registros<br/>(El Diario)"]
    Obs --> Traces["🔗 Trazas<br/>(La Narrativa)"]
    Obs --> Metrics["📊 Métricas<br/>(El Reporte de Salud)"]

    Logs --> L1["Eventos atómicos<br/>con timestamp"]
    Logs --> L2["Estructurados (JSON)<br/>con contexto completo:<br/>prompt, respuesta,<br/>entrada/salida de tools"]
    Logs --> L3["Ej: 'LLM llamado',<br/>'Herramienta X ejecutada',<br/>'Error 429 recibido'"]

    Traces --> T1["Conexión causal<br/>entre registros"]
    Traces --> T2["Trace ID único que<br/>sigue una solicitud<br/>de principio a fin"]
    Traces --> T3["Basado en OpenTelemetry<br/>(estándar de la industria)"]

    Metrics --> M1["Para Operaciones/SRE:<br/>P50/P99 latencia,<br/>tasa de error,<br/>costo por tarea"]
    Metrics --> M2["Para Ciencia de Datos/PM:<br/>tasa de acierto,<br/>adherencia a trayectoria,<br/>puntuación de utilidad"]

    style Obs fill:#e1f5fe,stroke:#333
    style Logs fill:#bbdefb
    style Traces fill:#c8e6c9
    style Metrics fill:#fff9c4
```

### Muestreo Dinámico

No necesitas capturar el trace completo de cada solicitud exitosa:

| Escenario | ¿Qué capturar? |
|-----------|---------------|
| ✅ Solicitudes exitosas (operación normal) | 10% de muestreo |
| ❌ Solicitudes fallidas | 100% — siempre capturar el trace completo |
| 🆕 Versión nueva del agente | 100% temporal hasta validar estabilidad |

---

## El Volante de Calidad del Agente

```mermaid
flowchart LR
    Define["🎯 Definir<br/>Objetivos de Calidad<br/>(4 Pilares)"] --> Instr["🔧 Instrumentar<br/>el Agente<br/>(Observabilidad)"]
    Instr --> Eval["📐 Evaluar<br/>Proceso y Resultados<br/>(Sistema Híbrido)"]
    Eval --> Feedback["💡 Feedback Loop<br/>Fallos → Nuevos casos<br/>de prueba → Mejora"]
    Feedback --> Define

    style Define fill:#e3f2fd
    style Instr fill:#c8e6c9
    style Eval fill:#fff9c4
    style Feedback fill:#ffcdd2
```

> **Cada interacción en producción, especialmente cada fallo, retroalimenta la mejora continua del agente.**

---

## Resumen: Principios Clave

1. **Diseña para la evaluabilidad desde el día cero** — la calidad no es un paso final, es un pilar arquitectónico
2. **La trayectoria es la verdad** — mira el proceso, no solo el destino
3. **El humano es el árbitro final** — la automatización escala, pero los valores humanos definen qué es "bueno"
4. **Observabilidad es la base** — sin visibilidad interna, estás operando a ciegas
5. **La evaluación es un bucle continuo** — el Volante de Calidad convierte cada interacción en una oportunidad de mejora

---

## Navegación

- [[dia-3-context-engineering-sesiones-y-memoria]] ← Anterior: sesiones y memoria
- [[opcional-dia-4-livestream]] → Siguiente: Livestream Q&A del Día 4
- [[5-day-ai-agents-intensive-course-with-google]] ← Volver al índice
