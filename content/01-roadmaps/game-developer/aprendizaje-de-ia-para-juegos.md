# Aprendizaje de IA para juegos

## ¿Por qué aprendizaje en juegos?

La IA tradicional (FSM, Behavior Trees) se programa **a mano**. La IA de aprendizaje **encuentra patrones por sí misma** a partir de datos o experiencia.

```mermaid
flowchart LR
    TRAD["IA Tradicional<br/>Programada a mano<br/>Predecible"] --> GAME["🎮"]
    ML["IA con Aprendizaje<br/>Entrenada con datos<br/>Adaptativa"] --> GAME
```

**¿Dónde se usa aprendizaje en juegos?**

```mermaid
flowchart TD
    AP["Aprendizaje en juegos"] --> BOTS["Bots que aprenden a jugar<br/>(OpenAI Five, AlphaStar)"]
    AP --> PCG["Generación procedural<br/>de niveles con ML"]
    AP --> NPC["NPCs adaptativos<br/>que aprenden del jugador"]
    AP --> BALANCE["Balanceo automático<br/>de dificultad"]
    AP --> ANIM["Animación / Motion Matching<br/>con ML"]
```

---

## 1. Red de Neuronas Artificiales (RNA)

### 1.1. La neurona artificial

Inspirada en la neurona biológica:

```mermaid
flowchart LR
    subgraph "Neurona biológica"
        DEND["Dentritas<br/>(entradas)"] --> SOMA["Soma<br/>(procesa)"]
        SOMA --> AXON["Axón<br/>(salida)"]
    end
    
    subgraph "Neurona artificial"
        X1["x₁"] --> N["Σ (wx + b)"]
        X2["x₂"] --> N
        X3["x₃"] --> N
        N --> ACT["f(z)"] --> Y["y = salida"]
    end
```

**Fórmula:**

```
z = w₁·x₁ + w₂·x₂ + ... + wₙ·xₙ + b
y = f(z)

f = función de activación
w = pesos
b = bias (sesgo)
```

### 1.2. Funciones de activación

```mermaid
flowchart LR
    ACT2[Funciones de Activación] --> SIG["Sigmoid<br/>σ(z) = 1/(1+e⁻ᶻ)<br/>Salida: [0, 1]"]
    ACT2 --> TANH["Tanh<br/>tanh(z)<br/>Salida: [-1, 1]"]
    ACT2 --> RELU["ReLU<br/>max(0, z)<br/>Salida: [0, ∞)"]
    ACT2 --> SOFT["Softmax<br/>eᶻⁱ / Σ eᶻʲ<br/>Probabilidades"]
```

### 1.3. Arquitectura de una red

```mermaid
flowchart TD
    subgraph "Red Feed-Forward"
        IN["Input Layer<br/>x₁, x₂, x₃"] --> H1["Hidden Layer 1<br/>64 neuronas"]
        H1 --> H2["Hidden Layer 2<br/>32 neuronas"]
        H2 --> OUT["Output Layer<br/>y₁, y₂"]
    end
```

**Ejemplo:** Clasificar si un enemigo debe atacar o huir.

```
Input:  [distancia_al_jugador, salud_propia, salud_jugador, municion]
Output: [atacar_prob, huir_prob, patrullar_prob]
```

### 1.4. Forward pass (inferencia)

```
float[] forward(float[] input, Network net) {
    float[] current = input;
    
    for (Layer layer : net.layers) {
        float[] next = new float[layer.size];
        
        for (int j = 0; j < layer.size; j++) {
            float sum = layer.biases[j];
            for (int i = 0; i < current.length; i++) {
                sum += current[i] * layer.weights[i][j];
            }
            next[j] = activation(sum);  // ReLU, sigmoid, etc.
        }
        current = next;
    }
    
    return current;  // Output
}
```

### 1.5. Backpropagation (entrenamiento)

```mermaid
flowchart LR
    FORWARD["Forward: calcular salida<br/>con pesos actuales"] --> LOSS["Loss: error<br/>entre salida y esperado"]
    LOSS --> BACK["Backward: gradiente<br/>del error por capas"]
    BACK --> UPDATE["Update: ajustar pesos<br/>w = w - α·∂L/∂w"]
    UPDATE --> FORWARD
```

---

## 2. Aprendizaje Profundo (Deep Learning)

### 2.1. ¿Qué lo hace "profundo"?

Múltiples **capas ocultas** (hidden layers) que aprenden representaciones jerárquicas:

```
Capa 1: bordes y texturas simples
Capa 2: formas (círculos, rectángulos)
Capa 3: partes de objetos (ojos, ruedas)
Capa 4: objetos completos (coche, persona)
```

```mermaid
flowchart TD
    INPUT["Input: píxeles"] --> C1["C1: bordes"]
    C1 --> C2["C2: formas"]
    C2 --> C3["C3: partes"]
    C3 --> C4["C4: objetos"]
    C4 --> OUT2["Output: ¿qué hay en la imagen?"]
```

### 2.2. Arquitecturas clave para juegos

| Arquitectura | Para qué |
|---|---|
| **CNN** (Convolucional) | Visión por computadora, juegos desde píxeles |
| **RNN / LSTM** | Secuencias, comportamiento temporal, diálogos |
| **Transformer** | Modelos grandes, decisiones complejas |
| **GAN** | Generación de texturas, niveles, assets |

### 2.3. Ejemplo: CNN para jugar desde píxeles

```mermaid
flowchart LR
    SCREEN["🎮 Pantalla<br/>84×84×4"] --> CONV["Conv Layers<br/>Extraer características"]
    CONV --> FC["Fully Connected<br/>Decidir acción"]
    FC --> ACTION["Acción:<br/>izquierda/derecha/salto"]
```

---

## 3. Aprendizaje de Refuerzo (Reinforcement Learning)

### 3.1. El paradigma

Un **agente** aprende a través de **prueba y error**, maximizando una **recompensa**.

```mermaid
flowchart LR
    AGENT["🤖 Agente"] --> ACTION2["Acción a_t"]
    ACTION2 --> ENV["🌍 Entorno (juego)"]
    ENV --> STATE["Estado s_t+1"]
    ENV --> REWARD["Recompensa r_t"]
    STATE --> AGENT
    REWARD --> AGENT
```

**Componentes:**
```
Estado (s)    → la situación actual del juego (posición, salud, puntuación)
Acción (a)    → lo que el agente puede hacer (saltar, disparar, moverse)
Recompensa (r)→ feedback: +1 por matar enemigo, -1 por recibir daño
Política (π)  → la estrategia: π(s) → a (qué acción tomar en cada estado)
```

### 3.2. Q-Learning (aprender valores de acción)

```
Q(s, a) = valor esperado de tomar acción a en estado s

Actualización:
Q(s, a) ← Q(s, a) + α·[r + γ·max Q(s', a') - Q(s, a)]

α = learning rate (qué tan rápido aprende)
γ = discount factor (qué tanto valora recompensas futuras)
```

### 3.3. Deep Q-Network (DQN)

Usa una **red neuronal** para aproximar Q(s, a). Famoso por jugar juegos de Atari solo viendo los píxeles.

```
// DQN simplificado
Input:  state (4 frames del juego, 84×84×4)
Output: Q-values para cada acción (disparar, saltar, mover...)

Loss = (r + γ·max Q(s', a') - Q(s, a))²  // Diferencia al cuadrado
```

```mermaid
flowchart TD
    TRAINING["Entrenamiento DQN"] --> PLAY["1. Jugar con política ε-greedy<br/>(aleatorio vs Q actual)"]
    PLAY --> MEM["2. Guardar experiencia<br/>(s, a, r, s') en buffer"]
    MEM --> SAMPLE["3. Samplear batch<br/>del buffer"]
    SAMPLE --> UPDATE["4. Actualizar Q-network<br/>con el loss"]
    UPDATE --> PLAY
```

### 3.4. Policy Gradients (PPO, A3C)

En lugar de aprender Q(s,a), aprenden directamente la **política π(s) → a**.

```
Ventajas de Policy Gradients:
- Acciones continuas (ángulo de giro, fuerza)
- Políticas estocásticas (exploración natural)
- Mejor convergencia en problemas complejos

PPO (Proximal Policy Optimization): el estándar moderno
Estable, eficiente, usado en OpenAI Five, etc.
```

### 3.5. Aplicaciones en juegos

```mermaid
flowchart TD
    RL["Reinforcement Learning"] --> AI5["OpenAI Five<br/>Dota 2 (2019)<br/>Derrota a pros"]
    RL --> ALPHA["AlphaGo / AlphaZero<br/>Go, Ajedrez, Shogi<br/>Supera campeones"]
    RL --> ALPHAS["AlphaStar<br/>StarCraft II<br/>Estrategia en tiempo real"]
    RL --> GT["Game Tuning<br/>Ajuste de dificultad<br/>Balance de personajes"]
    RL --> BOTS2["Bots adaptativos<br/>NPCs que aprenden<br/>del jugador"]
```

---

## 4. Aprendizaje por Árboles de Decisión

### 4.1. Concepto

El árbol se **construye automáticamente** a partir de datos de entrenamiento, dividiendo el espacio en regiones.

```mermaid
flowchart TD
    ROOT2{"¿Distancia al jugador?"}
    ROOT2 -->|"< 10m"| N1{"¿Salud del enemigo?"}
    ROOT2 -->|">= 10m"| LEAF1["Patrullar"]
    
    N1 -->|"< 30%"| LEAF2["Huir"]
    N1 -->|">= 30%"| N2{"¿Munición?"}
    
    N2 -->|"0"| LEAF3["Recargar"]
    N2 -->|"> 0"| LEAF4["Atacar"]
```

### 4.2. Cómo se construye

```
Algoritmo ID3 / C4.5 / CART:

1. Empezar con todos los datos en la raíz
2. Para cada atributo:
   - Calcular "ganancia de información" (entropía)
3. Elegir el atributo con mayor ganancia
4. Dividir los datos según ese atributo
5. Repetir recursivamente para cada subconjunto
6. Parar cuando todos los ejemplos son de la misma clase
```

**Entropía:** mide el desorden/incerteza:

```
Entropía(S) = -Σ pᵢ · log₂(pᵢ)

Ejemplo: si hay 50% atacar, 50% huir → entropía = 1 (máxima incerteza)
         si hay 100% atacar → entropía = 0 (certeza total)
```

### 4.3. Random Forest

No un árbol, sino un **bosque** de árboles entrenados con variaciones aleatorias:

```mermaid
flowchart LR
    DATA["Datos"] --> T1["Árbol 1"]
    DATA --> T2["Árbol 2"]
    DATA --> T3["Árbol 3"]
    DATA --> TN["... Árbol N"]
    T1 --> VOTE["Votación<br/>mayoría"]
    T2 --> VOTE
    T3 --> VOTE
    TN --> VOTE
    VOTE --> DECISION["Decisión final"]
```

**Ventaja:** Mucho más preciso que un solo árbol. Menos overfitting.

---

## 5. Clasificador Naive Bayes

### 5.1. Concepto

Clasificador **probabilístico** basado en el Teorema de Bayes. Asume que todas las características son **independientes** (naive = ingenuo).

```
Teorema de Bayes:
P(Clase | Evidencia) = P(Evidencia | Clase) · P(Clase) / P(Evidencia)

En clasificación:
P(y | x₁, x₂, ..., xₙ) ∝ P(y) · Π P(xᵢ | y)

y = clase a predecir
xᵢ = características
```

### 5.2. Ejemplo: Predecir comportamiento enemigo

```
Clases: {Atacar, Huir, Patrullar}
Características: {Distancia, Salud, Munición}

Datos de entrenamiento:
┌──────────┬────────┬───────┬───────────┐
│ Distancia │ Salud │ Munic │ Comportam │
├──────────┼────────┼───────┼───────────┤
│ Cerca    │ Alta   │ Sí    │ Atacar    │
│ Lejos    │ Baja   │ No    │ Huir      │
│ Media    │ Media  │ Sí    │ Patrullar │
│ Cerca    │ Baja   │ Sí    │ Huir      │
│ ...      │ ...    │ ...   │ ...       │
└──────────┴────────┴───────┴───────────┘

Nuevo caso: Distancia=Cerca, Salud=Baja, Munición=Sí
¿Qué comportamiento?

P(Atacar | Cerca, Baja, Sí) ∝ P(Atacar) · P(Cerca|Atacar) · P(Baja|Atacar) · P(Sí|Atacar)
P(Huir | Cerca, Baja, Sí)   ∝ P(Huir) · P(Cerca|Huir) · P(Baja|Huir) · P(Sí|Huir)
P(Patrullar | Cerca, Baja, Sí) ∝ P(Patrullar) · P(Cerca|Pat) · P(Baja|Pat) · P(Sí|Pat)

La clase con mayor probabilidad → comportamiento elegido
```

### 5.3. Aplicaciones

| Aplicación | Características | Clases |
|---|---|---|
| **Detección de tramposos** | Tasa de acierto, tiempo de reacción, movimientos | {Tramposo, Legítimo} |
| **Predicción de abandono** | Tiempo jugado, nivel, compras | {Abandona, Sigue} |
| **Matchmaking** | Habilidad, región, idioma | {Buena partida, Mala partida} |
| **Clasificación de jugadores** | Estilo de juego, acciones | {Agresivo, Defensivo, Explorador} |

### 5.4. Pros y Contras

| ✅ | ❌ |
|---|---|
| Muy rápido (lineal en características) | Asume independencia (rara vez cierta) |
| Funciona con pocos datos | Sensible a características irrelevantes |
| Probabilístico (da confianza) | No captura interacciones |
| Fácil de implementar | Datos continuos requieren discretización |

---

## 6. Aprendizaje de Decisiones

### 6.1. Concepto general

**Aprendizaje de decisiones** es el campo que estudia cómo los agentes pueden aprender a tomar decisiones óptimas a partir de experiencia. Engloba todas las técnicas anteriores.

```mermaid
flowchart TD
    AD[Aprendizaje de Decisiones] --> SUP["Supervisado<br/>Datos etiquetados<br/>(árboles, Bayes, redes)"]
    AD --> UNSUP["No supervisado<br/>Sin etiquetas<br/>(clustering, K-means)"]
    AD --> RL2["Por Refuerzo<br/>Prueba y error<br/>(Q-Learning, PPO)"]
    AD --> SEMI["Semi-supervisado<br/>Pocas etiquetas<br/>(combinación)"]
```

### 6.2. Enfoque supervisado para decisiones

```
Datos: (estado, acción_óptima) → el agente aprende a imitar

Fuentes de datos:
- Demos de jugadores humanos
- Registros de partidas
- Expertos jugando

El modelo aprende: dado este estado → ¿qué haría un experto?
```

### 6.3. Behavioral Cloning

Forma más simple: grabar a un humano jugando y entrenar una red para imitarlo.

```mermaid
flowchart LR
    HUMAN["🧑 Jugador humano"] --> REC["Grabar partidas<br/>(estado → acción)"]
    REC --> TRAIN["Entrenar red<br/>(imitar acciones)"]
    TRAIN --> BOT["🤖 Bot clonado<br/>(juega como el humano)"]
```

**Problema:** El bot comete errores que el humano nunca cometió (distribución shift). Solución: **DAgger** (Dataset Aggregation) o RL fine-tuning.

---

## 7. Tabla comparativa

| Técnica | Tipo | Datos necesarios | Velocidad runtime | Uso en juegos |
|---|---|---|---|---|
| **Red Neuronal (MLP)** | Supervisado | Muchos | Rápida (inferencia) | Clasificación, control |
| **Deep Learning (CNN/RNN)** | Supervisado | Muchísimos | Media (GPU) | Visión, secuencias |
| **Q-Learning / DQN** | Refuerzo | Experiencia | Rápida | Bots que aprenden |
| **PPO (Policy Gradient)** | Refuerzo | Experiencia | Rápida | Bots complejos (Dota, Starcraft) |
| **Árboles Decisión** | Supervisado | Pocos/Medios | Muy rápida | Clasificación simple |
| **Random Forest** | Supervisado | Medios | Rápida | Clasificación robusta |
| **Naive Bayes** | Supervisado | Pocos | Muy rápida | Clasificación probabilística |
| **Behavioral Cloning** | Imitativo | Demos | Rápida | Clonar estilo humano |

### ¿Cuándo usar qué?

```mermaid
flowchart TD
    Q2["¿Tienes datos?"] -->|"No"| RL3["Aprendizaje por Refuerzo<br/>(el agente aprende solo)"]
    Q2 -->|"Sí, pocos"| BAYES["Naive Bayes<br/>o Árboles de Decisión"]
    Q2 -->|"Sí, muchos"| DEEP["Deep Learning<br/>(CNN para visión,<br/>PPO/DQN para control)"]
    Q2 -->|"Sí, demos humanas"| CLONE["Behavioral Cloning<br/>+ RL fine-tuning"]
```

> 💡 **Regla de oro:** Si puedes programar la decisión a mano, hazlo (FSM, Behavior Tree). El aprendizaje es para cuando **no sabes la regla exacta** o quieres que la IA se **adapte** al jugador. Para la mayoría de juegos, RL + Behavioral Cloning es el combo más práctico.

## Relacionados:
- [[ia-para-juegos]] #anterior 