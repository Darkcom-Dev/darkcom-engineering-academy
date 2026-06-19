# Machine Learning

El **Machine Learning (ML)** es una rama de la Inteligencia Artificial que permite a las computadoras **aprender patrones** de los datos sin ser programadas explícitamente para cada tarea. En lugar de escribir regñas manuales, el algoritmo "aprende" de ejemplos.

```mermaid
flowchart LR
    A["📥 Datos históricos"] --> B["🤖 Algoritmo de ML"]
    B --> C["📐 Modelo entrenado"]
    C --> D["🔮 Predicciones"]
    
    style A fill:#3498db,color:#fff
    style B fill:#e67e22,color:#fff
    style C fill:#9b59b6,color:#fff
    style D fill:#2ecc71,color:#fff
```

### ML vs Programación tradicional

```mermaid
flowchart TD
    subgraph "Programación Tradicional"
        TRAD["Reglas + Datos → Respuestas"]
    end
    
    subgraph "Machine Learning"
        ML["Datos + Respuestas → Reglas (modelo)"]
    end
    
    style TRAD fill:#3498db,color:#fff
    style ML fill:#e74c3c,color:#fff
```

---

## Tipos de Machine Learning

```mermaid
flowchart TD
    ML["🤖 Machine Learning"]
    
    ML --> SUP["Supervisado<br/>✅ Aprende con ejemplos etiquetados"]
    ML --> UNSUP["No Supervisado<br/>🔍 Encuentra patrones sin etiquetas"]
    ML --> RL["Reforzado<br/>🎮 Aprende por prueba y error"]
    
    SUP --> SUP1["Regresión (predecir número)"]
    SUP --> SUP2["Clasificación (predecir categoría)"]
    
    UNSUP --> UNS1["Clustering (agrupar)"]
    UNSUP --> UNS2["Reducción de dimensionalidad"]
    
    RL --> RL1["Agente + Entorno<br/>+ Recompensa"]
    
    style ML fill:#3498db,color:#fff
    style SUP fill:#2ecc71,color:#fff
    style UNSUP fill:#e67e22,color:#fff
    style RL fill:#e74c3c,color:#fff
```

### Aprendizaje Supervisado

El modelo aprende de datos **etiquetados** (ya sabes la respuesta correcta) para predecir respuestas en datos nuevos.

| Tipo | ¿Qué predice? | Ejemplos |
|------|--------------|----------|
| **Regresión** | Un número | Precio de casa, temperatura, ventas |
| **Clasificación** | Una categoría | Spam / No spam, Perro / Gato, Enfermo / Sano |

```mermaid
flowchart LR
    subgraph "Supervisado"
        A["📥 Datos con etiquetas<br/>X: tamaño, habitaciones<br/>Y: precio"] --> B["🤖 Entrenar modelo"]
        B --> C["📐 Modelo"]
        C --> D["🔮 Predecir precio<br/>de casa nueva"]
    end
    
    style A fill:#3498db,color:#fff
    style B fill:#e67e22,color:#fff
    style C fill:#9b59b6,color:#fff
    style D fill:#2ecc71,color:#fff
```

### Aprendizaje No Supervisado

El modelo encuentra **patrones ocultos** en datos **sin etiquetas**. No sabe la respuesta, busca estructura.

```mermaid
flowchart LR
    subgraph "No Supervisado"
        A["📥 Datos sin etiquetas<br/>Clientes con edad, ingreso,<br/>compras..."] --> B["🔍 Algoritmo<br/>(Clustering)"]
        B --> C["📊 Grupos descubiertos<br/>Segmento A: jóvenes<br/>Segmento B: familias<br/>Segmento C: premium"]
    end
    
    style A fill:#3498db,color:#fff
    style B fill:#e67e22,color:#fff
    style C fill:#2ecc71,color:#fff
```

### Aprendizaje Reforzado

Un **agente** aprende a tomar decisiones interactuando con un **entorno**, recibiendo **recompensas** o **castigos** por sus acciones.

```mermaid
flowchart LR
    AGENT["🤖 Agente"] --> ACTION["🎮 Acción"]
    ACTION --> ENV["🌍 Entorno"]
    ENV --> REWARD["🏆 Recompensa"]
    ENV --> STATE["📊 Nuevo estado"]
    STATE --> AGENT
    REWARD --> AGENT
    
    style AGENT fill:#3498db,color:#fff
    style ACTION fill:#e67e22,color:#fff
    style ENV fill:#1abc9c,color:#fff
    style REWARD fill:#f39c12,color:#000
    style STATE fill:#9b59b6,color:#fff
```

---

## Algoritmos populares de ML

```mermaid
flowchart TD
    ALG["🤖 Algoritmos populares"]
    
    ALG --> SUPERVISED["Supervisados"]
    ALG --> UNSUPERVISED["No Supervisados"]
    
    SUPERVISED --> LR["Regresión Lineal<br/>(predecir números)"]
    SUPERVISED --> LOGREG["Regresión Logística<br/>(clasificación binaria)"]
    SUPERVISED --> TREES["Árboles de Decisión"]
    SUPERVISED --> KNN["KNN<br/>(vecinos cercanos)"]
    SUPERVISED --> NB["Naive Bayes"]
    SUPERVISED --> RF["Random Forest<br/>(muchos árboles)"]
    
    UNSUPERVISED --> KMEANS["K-Means<br/>(clustering)"]
    UNSUPERVISED --> PCA["PCA<br/>(reducción de dim.)"]
    
    style ALG fill:#3498db,color:#fff
    style SUPERVISED fill:#2ecc71,color:#fff
    style UNSUPERVISED fill:#e67e22,color:#fff
```

### Árboles de Decisión

Un árbol que **divide** los datos en ramas según preguntas sí/no, como un juego de "20 preguntas".

```mermaid
flowchart TD
    A["📊 ¿Ingreso > $50K?"]
    
    A -->|Sí| B["¿Edad > 30?"]
    A -->|No| C["¿Tiene tarjeta?"]
    
    B -->|Sí| D["✅ Compra"]
    B -->|No| E["❌ No compra"]
    
    C -->|Sí| F["✅ Compra"]
    C -->|No| G["❌ No compra"]
    
    style A fill:#e74c3c,color:#fff
    style B fill:#e67e22,color:#fff
    style C fill:#e67e22,color:#fff
```

**Ventajas:** Fácil de entender e interpretar (árbol visual).
**Desventajas:** Propenso a overfitting (se aprende los datos de memoria).

```python
from sklearn.tree import DecisionTreeClassifier

modelo = DecisionTreeClassifier(max_depth=3)  # limita profundidad
modelo.fit(X_train, y_train)
predicciones = modelo.predict(X_test)
```

---

### Naive Bayes

Clasificador basado en el **Teorema de Bayes**. Asume que todas las variables son **independientes** entre sí (de ahí lo de "naive" — ingenuo).

**Ideal para:** Clasificación de texto, detección de spam, análisis de sentimientos.

```python
from sklearn.naive_bayes import GaussianNB

modelo = GaussianNB()
modelo.fit(X_train, y_train)
predicciones = modelo.predict(X_test)
```

A pesar de su supuesto "ingenuo", funciona sorprendentemente bien en la práctica, especialmente con texto.

---

### KNN (K-Nearest Neighbors)

Clasifica un punto según la **mayoría de sus K vecinos más cercanos**.

```mermaid
flowchart LR
    subgraph "KNN con K=3"
        A["🟦 Clase A"] --- B["🟦 Clase A"]
        B --- C["🟥 Clase B"]
        C --- D["❓ ¿Nuevo punto?"]
        D -.-> A
        D -.-> B
        D -.-> C
    end
    
    A2["Resultado:<br/>2 vecinos A + 1 vecino B<br/>→ Clasificado como A 🟦"]
    
    style A2 fill:#2ecc71,color:#fff
```

**Ideal para:** Cuando los datos forman grupos naturales.

**Desventaja:** No escala bien con muchos datos ni muchas dimensiones.

```python
from sklearn.neighbors import KNeighborsClassifier

modelo = KNeighborsClassifier(n_neighbors=5)
modelo.fit(X_train, y_train)
predicciones = modelo.predict(X_test)
```

---

### K-Means Clustering

Algoritmo **no supervisado** que agrupa datos en **K grupos** basándose en su cercanía.

```mermaid
flowchart TD
    IN["📥 Datos sin etiquetas"] --> K["Elegir K<br/>(ej: 3 grupos)"]
    K --> INIT["Inicializar centros<br/>aleatoriamente"]
    INIT --> ASSIGN["Asignar cada punto<br/>al centro más cercano"]
    ASSIGN --> UPDATE["Recalcular centros<br/>(promedio del grupo)"]
    UPDATE --> CHECK{"¿Cambiaron<br/>los centros?"}
    CHECK -->|Sí| ASSIGN
    CHECK -->|No| DONE["✅ Grupos finales"]
    
    style IN fill:#3498db,color:#fff
    style K fill:#e67e22,color:#fff
    style DONE fill:#2ecc71,color:#fff
```

**Ideal para:** Segmentación de clientes, compresión de imágenes, análisis exploratorio.

```python
from sklearn.cluster import KMeans

modelo = KMeans(n_clusters=3, random_state=42)
modelo.fit(X)
df["cluster"] = modelo.labels_
```

> **¿Cómo elegir K?** Usa el **método del codo (elbow method)**: grafica la inercia vs K y busca el punto donde la mejora se ralentiza.

---

### Regresión Logística

A pesar del nombre, es un algoritmo de **clasificación** (no regresión). Predice la **probabilidad** de que un caso pertenezca a una clase.

```
P(Y=1) = 1 / (1 + e^-(β₀ + β₁·X))
```

```python
from sklearn.linear_model import LogisticRegression

modelo = LogisticRegression()
modelo.fit(X_train, y_train)
predicciones = modelo.predict(X_test)

# Probabilidades
probabilidades = modelo.predict_proba(X_test)[:, 1]
```

**Ideal para:** Clasificación binaria (sí/no, compra/no compra, enfermo/sano).

---

## Pipeline completo de ML

```mermaid
flowchart TD
    RAW["📥 Datos crudos"] --> CLEAN["🧹 Limpiar datos"]
    CLEAN --> EDA["🔍 EDA / Visualizar"]
    EDA --> PREP["🔧 Preparar datos<br/>- Dividir en X/y<br/>- Train/Test split"]
    PREP --> CHOOSE["🎯 Elegir algoritmo"]
    CHOOSE --> TRAIN["🤖 Entrenar modelo"]
    TRAIN --> EVAL["📊 Evaluar modelo"]
    EVAL --> TUNE{"¿Resultados<br/>aceptables?"}
    TUNE -->|No| CHOOSE
    TUNE -->|No| PREP
    TUNE -->|Sí| DEPLOY["🚀 Desplegar / Predecir"]
    
    style RAW fill:#ff6b6b,color:#fff
    style CLEAN fill:#e74c3c,color:#fff
    style EDA fill:#f39c12,color:#000
    style PREP fill:#3498db,color:#fff
    style CHOOSE fill:#9b59b6,color:#fff
    style TRAIN fill:#e67e22,color:#fff
    style EVAL fill:#1abc9c,color:#fff
    style DEPLOY fill:#2ecc71,color:#fff
```

---

## Técnicas de evaluación de modelos

¿Cómo saber si tu modelo es bueno? Divides los datos en **entrenamiento** y **prueba**, entrenas con unos y evaluas con otros.

```mermaid
flowchart LR
    DATA["📊 Todos los datos"] --> SPLIT["✂️ División"]
    
    SPLIT --> TRAIN["🎯 80% Entrenamiento<br/>El modelo aprende aquí"]
    SPLIT --> TEST["🔍 20% Prueba<br/>El modelo se evalúa aquí"]
    
    TRAIN --> MODEL["🤖 Modelo entrenado"]
    MODEL --> PRED["🔮 Predicciones sobre test"]
    TEST --> PRED
    PRED --> METRICS["📊 Métricas de evaluación"]
    
    style DATA fill:#3498db,color:#fff
    style TRAIN fill:#2ecc71,color:#fff
    style TEST fill:#e74c3c,color:#fff
    style METRICS fill:#9b59b6,color:#fff
```

### Métricas para clasificación

```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, confusion_matrix

y_pred = modelo.predict(X_test)

# Métricas principales
print(f"Accuracy:  {accuracy_score(y_test, y_pred):.3f}")
print(f"Precision: {precision_score(y_test, y_pred):.3f}")
print(f"Recall:    {recall_score(y_test, y_pred):.3f}")
print(f"F1-Score:  {f1_score(y_test, y_pred):.3f}")

# Matriz de confusión
print(confusion_matrix(y_test, y_pred))
```

#### Matriz de Confusión

```mermaid
flowchart TD
    subgraph "Matriz de Confusión"
        A[" "] --- B[" "] --- C["Predicho: NO"] --- D["Predicho: SÍ"]
        E["Real: NO"] --- F[" "] --- G["✅ Verdadero Negativo<br/>(VN)"] --- H["❌ Falso Positivo<br/>(FP)"]
        I["Real: SÍ"] --- J[" "] --- K["❌ Falso Negativo<br/>(FN)"] --- L["✅ Verdadero Positivo<br/>(VP)"]
    end
    
    style G fill:#2ecc71,color:#fff
    style L fill:#2ecc71,color:#fff
    style H fill:#e74c3c,color:#fff
    style K fill:#e74c3c,color:#fff
```

| Métrica | Fórmula | ¿Qué mide? |
|---------|---------|-----------|
| **Accuracy** | (VP + VN) / Total | ¿Qué tan seguido acierta el modelo? |
| **Precision** | VP / (VP + FP) | De los que predijo como SÍ, ¿cuántos eran realmente SÍ? |
| **Recall** | VP / (VP + FN) | De los SÍ reales, ¿cuántos detectó? |
| **F1-Score** | 2 × (P × R) / (P + R) | Balance entre precisión y recall |

> 💡 **¿Cuál mirar?** Depende del problema. En detección de spam, prefiere **precision** (no marcar correos buenos como spam). En detección de cáncer, prefiere **recall** (no dejar pasar un caso real).

### Métricas para regresión

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

y_pred = modelo.predict(X_test)

print(f"MAE:  {mean_absolute_error(y_test, y_pred):.3f}")
print(f"MSE:  {mean_squared_error(y_test, y_pred):.3f}")
print(f"RMSE: {mean_squared_error(y_test, y_pred, squared=False):.3f}")
print(f"R²:   {r2_score(y_test, y_pred):.3f}")
```

| Métrica | ¿Qué mide? | Rango ideal |
|---------|-----------|-------------|
| **MAE** | Error promedio absoluto | Lo más bajo posible |
| **MSE** | Error cuadrático (penaliza más errores grandes) | Lo más bajo posible |
| **RMSE** | Raíz del MSE (misma unidad que Y) | Lo más bajo posible |
| **R²** | Proporción de varianza explicada | 0 a 1 (más alto = mejor) |

### Overfitting y Underfitting

```mermaid
flowchart LR
    subgraph "Underfitting<br/>(muy simple)"
        A["📈 _/¯_ "]
    end
    
    subgraph "¡Balance ideal!"
        B["📈 ╱╲ "]
    end
    
    subgraph "Overfitting<br/>(muy complejo)"
        C["📈 ~\/~\/~ "]
    end
    
    style A fill:#e74c3c,color:#fff
    style B fill:#2ecc71,color:#fff
    style C fill:#e74c3c,color:#fff
```

| Problema | Síntoma | Solución |
|----------|---------|----------|
| **Underfitting** | Modelo muy simple, no aprende ni en training | Aumentar complejidad, más features, menos regularización |
| **Overfitting** | Modelo muy complejo, memoriza training pero falla en test | Regularización, más datos, reducir features, podar árbol |

### Validación Cruzada (Cross-Validation)

En lugar de un solo split train/test, divide los datos en **K partes** y entrena K veces, usando cada parte como test una vez:

```python
from sklearn.model_selection import cross_val_score

# Validación cruzada con 5 pliegues
scores = cross_val_score(modelo, X, y, cv=5)
print(f"Accuracy promedio: {scores.mean():.3f} (+/- {scores.std()*2:.3f})")
```

```mermaid
flowchart TD
    DATA["📊 Todos los datos"] --> FOLD1["Fold 1: 🟩🟩🟩🟩🟥"]
    DATA --> FOLD2["Fold 2: 🟩🟩🟩🟥🟩"]
    DATA --> FOLD3["Fold 3: 🟩🟩🟥🟩🟩"]
    DATA --> FOLD4["Fold 4: 🟩🟥🟩🟩🟩"]
    DATA --> FOLD5["Fold 5: 🟥🟩🟩🟩🟩"]
    
    FOLD1 --> R1["Entreno en 🟩, evalúo en 🟥 → Score 1"]
    FOLD2 --> R2["Entreno en 🟩, evalúo en 🟥 → Score 2"]
    FOLD3 --> R3["Entreno en 🟩, evalúo en 🟥 → Score 3"]
    FOLD4 --> R4["Entreno en 🟩, evalúo en 🟥 → Score 4"]
    FOLD5 --> R5["Entreno en 🟩, evalúo en 🟥 → Score 5"]
    
    R1 --> AVG["📊 Score promedio"]
    R2 --> AVG
    R3 --> AVG
    R4 --> AVG
    R5 --> AVG
    
    style DATA fill:#3498db,color:#fff
    style AVG fill:#2ecc71,color:#fff
```

---

## Algoritmo vs tipo de problema

```mermaid
flowchart TD
    Q["🎯 ¿Qué tipo de problema?"]
    
    Q --> REG["📈 Regresión<br/>(predecir número)"]
    REG --> REG1["Regresión Lineal<br/>Random Forest<br/>XGBoost"]
    
    Q --> CLASS["📋 Clasificación<br/>(predecir categoría)"]
    CLASS --> CLASS1["Regresión Logística<br/>Árbol de Decisión<br/>KNN, Naive Bayes<br/>Random Forest"]
    
    Q --> CLUST["🔍 Clustering<br/>(agrupar sin etiquetas)"]
    CLUST --> CLUST1["K-Means<br/>DBSCAN<br/>Hierarchical"]
    
    Q --> REDUC["📉 Reducción de dim.<br/>(simplificar datos)"]
    REDUC --> REDUC1["PCA<br/>t-SNE"]
    
    style Q fill:#3498db,color:#fff
    style REG1 fill:#2ecc71,color:#fff
    style CLASS1 fill:#2ecc71,color:#fff
    style CLUST1 fill:#2ecc71,color:#fff
    style REDUC1 fill:#2ecc71,color:#fff
```

> 📁 **Ver también**: [[analisis-estadistico]], [[ganar-habilidades-de-programacion]], [[introduccion-analisis-de-datos]]

## Relacionados:
- [[analisis-estadistico]] #anterior 
- [[tecnicas-de-big-data]] #siguiente 