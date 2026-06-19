# Mejorando la Confiabilidad

Los LLMs son poderosos pero **no son perfectos**. Pueden tener sesgos, ser inconsistentes o fallar en casos borde. Este documento cubre técnicas para hacer tus prompts más **robustos, justos y predecibles**.

```mermaid
flowchart TB
    Confiabilidad["🎯 Confiabilidad"] --> Debiasing["Desesgamiento<br/>Eliminar sesgos"]
    Confiabilidad --> Ensamble["Ensamblado<br/>Combinar respuestas"]
    Confiabilidad --> AutoEval["Autoevaluación<br/>El LLM se corrige"]
    Confiabilidad --> Calibracion["Calibración<br/>Ajustar confianza"]
    Debiasing --> Robusto["✅ Prompt robusto"]
    Ensamble --> Robusto
    AutoEval --> Robusto
    Calibracion --> Robusto
    style Confiabilidad fill:#e1f5fe,stroke:#333
    style Debiasing fill:#bbdefb
    style Ensamble fill:#c8e6c9
    style AutoEval fill:#fff9c4
    style Calibracion fill:#fce4ec
    style Robusto fill:#81c784,stroke:#333
```

---

## Desesgamiento de Prompts

Los LLMs aprenden de datos humanos, y los datos humanos contienen **sesgos**. El desesgamiento busca reducir o eliminar estos sesgos en las respuestas.

### Tipos de sesgo comunes

```mermaid
flowchart TB
    Sesgos["⚠️ Sesgos en LLMs"] --> Cultural["🌍 Cultural<br/>

Prioriza perspectivas<br/>occidentales"]
    Sesgos --> Genero["👫 Género<br/>
Asocia roles por<br/>estereotipos"]
    Sesgos --> Racial["🏽 Racial<br/>
Refleja prejuicios<br/>raciales"]
    Sesgos --> Confirmacion["✅ Confirmación<br/>
Tiende a estar de<br/>acuerdo contigo"]
    Sesgos --> Reciente["📅 Reciente<br/>
Sobrerrepresenta<br/>información actual"]
    Sesgos --> Positividad["😊 Positividad<br/>
Sesgo a respuestas<br/>optimistas"]

    style Sesgos fill:#ffcdd2
    style Cultural fill:#e3f2fd
    style Genero fill:#e8f5e9
    style Racial fill:#fff9c4
    style Confirmacion fill:#fce4ec
    style Reciente fill:#e8eaf6
    style Positividad fill:#fff3e0
```

### Ejemplos de sesgo

**Sesgo de género:**
```
❌ Prompt: "El médico recomendó una cirugía. ____ es muy experimentado."
    → El modelo completa con "Él" (asocia médico con masculino)

✅ Prompt neutro: "El médico recomendó una cirugía. El/La doctor(a)
    es muy experimentado(a). ¿Cuál es el pronóstico?"
    → Evita asumir género
```

**Sesgo de confirmación:**
```
❌ Prompt: "¿No crees que la programación funcional es el mejor enfoque?"
    → El modelo tiende a estar de acuerdo contigo

✅ Prompt: "¿Cuáles son las ventajas y desventajas de la programación
    funcional frente a la orientada a objetos?"
    → Fomenta una respuesta balanceada
```

### Técnicas de desesgamiento

| Técnica | Descripción | Ejemplo |
|---------|-------------|---------|
| **Instrucción explícita** | Pide neutralidad en el prompt | _"Sé objetivo y neutral"_ |
| **Contra-prompting** | Pide explícitamente considerar otras perspectivas | _"Considera también el punto de vista opuesto"_ |
| **Eliminar contexto sesgado** | No incluyas suposiciones en el prompt | En lugar de _"Como todo el mundo sabe..."_, ve directo a la pregunta |
| **Diversidad en ejemplos** | Usa ejemplos variados en few-shot | Nombres diversos, contextos variados |
| **Debiasing en system prompt** | Instrucción base contra sesgos | _"No asumas género, raza, religión..."_ |

```
Ejemplo de system prompt desesgado:

System:
  "Eres un asistente objetivo y neutral.
  - No asumas género, nacionalidad, raza o religión.
  - Si la pregunta tiene múltiples perspectivas, preséntalas todas.
  - No estés de acuerdo automáticamente con el usuario.
  - Basa tus respuestas en evidencia factual.
  - Si no hay consenso, indícalo."
```

### Cómo detectar sesgos

```mermaid
flowchart LR
    P["Prompt"] --> LLM["🤖 LLM"] --> R1["Respuesta 1"]
    P --> LLM --> R2["Respuesta 2<br/>temperatura alta"]
    R1 --> Audit["🔍 Auditoría<br/>¿Hay patrones<br/>sesgados?"]
    R2 --> Audit
    Audit -->|"Sí"| Fix["🔧 Rediseñar prompt<br/>Agregar instrucciones<br/>de neutralidad"]
    Audit -->|"No"| OK["✅ Prompt limpio"]
    style P fill:#bbdefb
    style LLM fill:#e8eaf6
    style R1 fill:#e3f2fd
    style R2 fill:#bbdefb
    style Audit fill:#fff9c4
    style Fix fill:#ffcdd2
    style OK fill:#c8e6c9
```

---

## Ensamblado de Prompts

Combinar **múltiples prompts o respuestas** para obtener un resultado más robusto que cualquiera individualmente.

```mermaid
flowchart TB
    subgraph Ensamblado
        P["Pregunta"] --> P1["Prompt variante 1"]
        P --> P2["Prompt variante 2"]
        P --> P3["Prompt variante 3"]
        P1 --> R1["Respuesta A"]
        P2 --> R2["Respuesta B"]
        P3 --> R3["Respuesta C"]
        R1 --> Ensamble["🧩 Ensamblar"]
        R2 --> Ensamble
        R3 --> Ensamble
        Ensamble --> Final["🏆 Respuesta final<br/>combinada"]
    end
    style P fill:#bbdefb
    style P1 fill:#e3f2fd
    style P2 fill:#bbdefb
    style P3 fill:#e3f2fd
    style Ensamble fill:#fff9c4
    style Final fill:#c8e6c9
```

### Estrategias de ensamblado

| Estrategia | Cómo funciona | Cuándo usarla |
|------------|--------------|---------------|
| **Votación** | Múltiples prompts, misma pregunta, mayoría gana | Clasificación, opción múltiple |
| **Promedio** | Varias respuestas numéricas, se promedian | Estimaciones, rangos |
| **Cascada** | Prompt 1 → limpia, Prompt 2 → refina, Prompt 3 → verifica | Tareas complejas de múltiples pasos |
| **Panel de expertos** | Cada prompt tiene un rol diferente, se consolida | Análisis desde múltiples perspectivas |

### Ejemplo: Votación

```
Prompt variante A:
  "Clasifica este email como spam (1) o no spam (0): {email}"

Prompt variante B:
  "Eres un filtro de correo. ¿Este email es spam? Responde SI o NO: {email}"

Prompt variante C:
  "Analiza el siguiente email y determina si es correo no deseado.
   Devuelve solo 'spam' o 'no_spam': {email}"

Resultados:
  A → spam
  B → no spam  ← error
  C → spam
  → Votación: spam (2/3) ✅
```

### Ejemplo: Panel de Expertos

```
System A: "Eres un analista financiero. Evalúa el riesgo de esta inversión."
System B: "Eres un abogado. Identifica problemas legales en esta propuesta."
System C: "Eres un experto en tecnología. Evalúa la viabilidad técnica."

→ Llama a los 3, consolida las respuestas en un reporte único.
```

```mermaid
flowchart TB
    Q["💼 ¿Deberíamos<br/>invertir en esta startup?"] --> Fin["📊 Analista<br/>Financiero"]
    Q --> Leg["⚖️ Abogado"]
    Q --> Tech["💻 Experto<br/>Técnico"]
    Fin --> R1["Rentabilidad: media<br/>Riesgo: alto"]
    Leg --> R2["Problemas legales:<br/>patente pendiente"]
    Tech --> R3["Stack técnico: sólido<br/>Escalabilidad: dudosa"]
    R1 --> Consolidador["🧩 Consolidar"]
    R2 --> Consolidador
    R3 --> Consolidador
    Consolidador --> Final["Reporte final<br/>multiperspectiva"]
    style Q fill:#bbdefb
    style Fin fill:#c8e6c9
    style Leg fill:#fce4ec
    style Tech fill:#e3f2fd
    style Consolidador fill:#fff9c4
    style Final fill:#81c784,stroke:#333
```

---

## LLM Autoevaluación

Hacer que el propio LLM **evalúe y critique su propia respuesta** o la de otro modelo.

```mermaid
flowchart TB
    P["Prompt"] --> Gen["🤖 Genera<br/>respuesta inicial"]
    Gen --> Eval["🔍 Autoevaluación<br/>'¿Esta respuesta es<br/>correcta? ¿Completa?<br/>¿Clara?'"]
    Eval --> Check{"¿Cumple<br/>criterios?"}
    Check -->|"✅ Sí"| Final["✅ Respuesta final"]
    Check -->|"❌ No"| Correccion["🔄 Corrige<br/>'Mejora la respuesta<br/>considerando: ...'"]
    Correccion --> Gen
    style P fill:#bbdefb
    style Gen fill:#e3f2fd
    style Eval fill:#fff9c4
    style Check fill:#ffcdd2
    style Final fill:#c8e6c9
    style Correccion fill:#e8eaf6
```

### Técnicas de autoevaluación

#### 1. Verificación en dos pasos

```
Paso 1 — Generar:
  "Responde: ¿Cuál es la capital de Australia?"

Paso 2 — Verificar:
  "Revisa tu respuesta anterior. ¿Estás seguro de que {respuesta}
   es la capital de Australia? Si no, corrígelo."
```

#### 2. Cadena de verificación

```
"Responde la pregunta. Luego, para cada afirmación en tu respuesta,
 indica si estás: (a) 100% seguro, (b) moderadamente seguro,
 (c) no seguro. Para (b) y (c), explica qué información adicional
 necesitarías para estar 100% seguro."
```

#### 3. Contra-pregunta

```
"Responde: ¿Qué es la teoría de cuerdas?

Después de responder, hazte esta contra-pregunta:
'¿Hay alguna visión alternativa o crítica a esta teoría que deba mencionar?'
Si la hay, agrégala a tu respuesta."
```

#### 4. Evaluación con rúbrica

```
Prompt de autoevaluación:
  "Evalúa tu respuesta anterior usando esta rúbrica:
   - Precisión (1-5): ¿Los hechos son correctos?
   - Claridad (1-5): ¿Se entiende fácilmente?
   - Completitud (1-5): ¿Cubre todos los aspectos?
   - Neutralidad (1-5): ¿Es objetiva?
   
   Si algún puntaje es menor a 4, genera una versión mejorada."
```

```mermaid
flowchart LR

    R["Respuesta"] --> Rubrica["Aplicar rúbrica"]

    Rubrica --> Scores["Puntajes<br/>Precisión: 3<br/>Claridad: 5<br/>Completitud: 2"]

    Scores --> Decision{"¿Todo >= 4?"}

    Decision -->|No| Mejora["Mejorar<br/>áreas débiles"]
    Mejora --> R

    Decision -->|Sí| OK["Aceptar"]

    style R fill:#e3f2fd
    style Rubrica fill:#fff9c4
    style Scores fill:#bbdefb
    style Mejora fill:#ffcdd2
    style OK fill:#c8e6c9
```

### Limitaciones de la autoevaluación

| ⚠️ Problema | Explicación |
|-------------|-------------|
| **Falso sentido de seguridad** | El modelo puede aprobar su propia respuesta incorrecta |
| **Ceguera a patrones** | Si el modelo tiene un sesgo, la autoevaluación también lo tendrá |
| **Costo adicional** | Duplica o triplica el número de llamadas al LLM |
| **Mejora marginal** | Para respuestas factuales, RAG o búsqueda externa suele ser más efectivo |

> 💡 **Regla:** la autoevaluación funciona mejor para **calidad de escritura, claridad y formato** que para **precisión factual**.

---

## Calibrando LLM

La calibración busca **ajustar la confianza del modelo** para que sus niveles de certeza reflejen la realidad.

```
Modelo mal calibrado:
  "Estoy 100% seguro de que la respuesta es..." → pero se equivoca el 30% de las veces

Modelo bien calibrado:
  Cuando dice "100% seguro" → acierta el 100% de las veces
  Cuando dice "70% seguro" → acierta ~70% de las veces
```

```mermaid
flowchart TB
    subgraph Mal calibrado
        Confianza["Confianza declarada"] --> Real["Precisión real"]
        C90["90% seguro"] --> R50["50% acierta"]
        C70["70% seguro"] --> R40["40% acierta"]
    end
    subgraph Bien calibrado
        C90b["90% seguro"] --> R90b["~90% acierta"]
        C70b["70% seguro"] --> R70b["~70% acierta"]
    end
    style Mal calibrado fill:#ffcdd2
    style Bien calibrado fill:#c8e6c9
```

### Técnicas de calibración

#### 1. Preguntar nivel de confianza explícitamente

```
"Responde la siguiente pregunta y luego indica tu nivel de confianza
 en la respuesta (0-100%):

 Pregunta: ¿Cuál es la fórmula química del agua?
 
 Explica brevemente tu razonamiento y luego:
 Confianza: ___%"
```

#### 2. Forzar al modelo a decir "No sé"

```
Prompt:
  "Si no estás 100% seguro de la respuesta, responde
  'No estoy seguro' en lugar de adivinar.

  Pregunta: {pregunta}"
```

#### 3. Múltiples muestras para medir consistencia

```python
def calibrar_respuesta(prompt, n_muestras=5):
    """Genera N respuestas y mide la consistencia."""
    respuestas = []
    for _ in range(n_muestras):
        resp = llm_call(prompt, temperature=0.3)
        respuestas.append(resp)
    
    consistencia = len(set(respuestas)) / len(respuestas)
    confianza_estimada = 1 - consistencia
    
    # Si todas son iguales → alta confianza
    # Si varían mucho → baja confianza
    return respuestas[0], confianza_estimada
```

#### 4. Prompt de calibración

```
"Eres un asistente calibrado. Para cada pregunta:
1. Responde solo si estás seguro
2. Si tienes dudas, indícalo
3. Califica tu certeza como: 'alta', 'media' o 'baja'

Pregunta: {pregunta}

Formato de respuesta:
Respuesta: ...
Certeza: [alta/media/baja]
Razonamiento: ..."
```

### Evaluación de calibración

```mermaid
flowchart LR
    Test["🧪 Conjunto de<br/>preguntas de prueba"] --> LLM["🤖 LLM responde<br/>con nivel de confianza"]
    LLM --> Compare["📊 Comparar<br/>confianza vs<br/>precisión real"]
    Compare --> Plot["📈 Curva de<br/>calibración"]
    Plot --> Gap{"¿Diferencia<br/>significativa?"}
    Gap -->|"Sí"| Adjust["🔧 Ajustar prompt<br/>o técnica"]
    Gap -->|"No"| Well["✅ Bien calibrado"]
    style Test fill:#bbdefb
    style LLM fill:#e3f2fd
    style Compare fill:#fff9c4
    style Plot fill:#e8eaf6
    style Gap fill:#ffcdd2
    style Adjust fill:#fce4ec
    style Well fill:#c8e6c9
```

### Errores frecuentes de calibración

| Error | Descripción | Ejemplo |
|-------|-------------|---------|
| **Sobreconfianza** | el modelo dice estar seguro cuando no debería | _"100% seguro"_ sobre un hecho inventado |
| **Subconfianza** | el modelo es inseguro incluso cuando acierta | _"Quizás..."_ en una respuesta correcta |
| **Confianza uniforme** | siempre da el mismo nivel de confianza | Todo es "moderadamente seguro" |
| **Confianza inversa** | más seguro cuando se equivoca | Errores graves presentados con alta certeza |

---

## Resumen Visual

```mermaid
flowchart TB
    title["📊 Confiabilidad de Prompts"] --> Debiasing
    title --> Ensamble
    title --> AutoEval
    title --> Calibracion

    subgraph Debiasing["Desesgamiento"]
        D1["Instrucción<br/>explícita"] --> D2["Contra-<br/>prompting"]
        D2 --> D3["Ejemplos<br/>diversos"]
    end

    subgraph Ensamble["Ensamblado"]
        E1["Votación"] --> E2["Panel de<br/>expertos"]
        E2 --> E3["Cascada"]
    end

    subgraph AutoEval["Autoevaluación"]
        A1["Dos pasos:<br/>generar + verificar"] --> A2["Rúbrica de<br/>evaluación"]
        A2 --> A3["Contra-<br/>pregunta"]
    end

    subgraph Calibracion["Calibración"]
        C1["Confianza<br/>explícita"] --> C2["Decir<br/>'No sé'"]
        C2 --> C3["Múltiples<br/>muestras"]
    end

    Debiasing --> Resultado["✅ Prompts más<br/>robustos"]
    Ensamble --> Resultado
    AutoEval --> Resultado
    Calibracion --> Resultado

    style title fill:#e1f5fe,stroke:#333
    style Debiasing fill:#bbdefb
    style Ensamble fill:#c8e6c9
    style AutoEval fill:#fff9c4
    style Calibracion fill:#fce4ec
    style Resultado fill:#81c784,stroke:#333
```

---

## Tabla Comparativa: ¿Qué técnica usar?

| Problema | Técnica recomendada | Esfuerzo | Impacto |
|----------|-------------------|----------|---------|
| **Sesgos en respuestas** | Desesgamiento | Bajo | Alto |
| **Respuestas inconsistentes** | Ensamblado (votación) | Medio | Muy alto |
| **Respuestas incompletas** | Autoevaluación | Medio | Alto |
| **Modelo demasiado confiado** | Calibración | Bajo | Medio |
| **Fallos en casos borde** | Ensamblado + Autoevaluación | Alto | Muy alto |
| **Necesito máxima precisión** | Las 4 combinadas | Alto | Máximo |

---

## Relacionados

- [[ingenieria-de-prompts]] ← Anterior: automatización y mejores prácticas
