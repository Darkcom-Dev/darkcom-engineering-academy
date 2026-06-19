# Análisis descriptivo

El análisis descriptivo responde a la pregunta **"¿Qué pasó?"**. Consiste en **resumir y describir** las características principales de un conjunto de datos usando medidas estadísticas y visualizaciones. Es el punto de partida de cualquier análisis.

```mermaid
flowchart LR
    A["📥 Datos"] --> B["📐 Describir"]
    B --> C["📊 Resumen"]
    C --> D["🔍 Insights"]
    
    style A fill:#3498db,color:#fff
    style B fill:#9b59b6,color:#fff
    style C fill:#f39c12,color:#000
    style D fill:#2ecc71,color:#fff
```

---

## Generando estadísticas

Las estadísticas descriptivas se dividen en **3 categorías** principales:

```mermaid
flowchart TD
    EST["📊 Estadísticas<br/>Descriptivas"]
    
    EST --> TC["📌 Tendencia Central<br/>¿Dónde está el centro?"]
    EST --> DISP["📏 Dispersión<br/>¿Qué tan dispersos están?"]
    EST --> FORMA["📈 Forma de distribución<br/>¿Cómo se distribuyen?"]
    
    TC --> TC1["Promedio (media)"]
    TC --> TC2["Mediana"]
    TC --> TC3["Moda"]
    
    DISP --> D1["Rango"]
    DISP --> D2["Varianza"]
    DISP --> D3["Desviación Estándar"]
    
    FORMA --> F1["Sesgo (Skewness)"]
    FORMA --> F2["Curtosis (Kurtosis)"]
    
    style EST fill:#3498db,color:#fff
    style TC fill:#e74c3c,color:#fff
    style DISP fill:#9b59b6,color:#fff
    style FORMA fill:#1abc9c,color:#fff
```

---

### Tendencia central

La **tendencia central** busca el valor "típico" o "central" de un conjunto de datos.

| Medida | Definición | ¿Cuándo usarla? |
|--------|-----------|-----------------|
| **Media (promedio)** | Suma de valores ÷ cantidad de valores | Datos simétricos, sin outliers |
| **Mediana** | Valor central cuando los datos están ordenados | Datos con outliers o asimétricos |
| **Moda** | Valor que más se repite | Datos categóricos o discretos |

#### Media (Promedio)

```
Media = (x₁ + x₂ + ... + xₙ) / n
```

```python
import pandas as pd

df["columna"].mean()
```

| Datos | Media |
|-------|-------|
| [10, 20, 30, 40, 50] | 30 |
| [10, 20, 30, 40, 1000] | 220 ← **outlier distorsiona** |

> ⚠️ La media es **sensible a outliers**. Un valor extremo la arrastra.

#### Mediana

La mediana es el valor que divide los datos en dos mitades iguales. No se ve afectada por outliers.

```python
df["columna"].median()
```

| Datos | Mediana |
|-------|---------|
| [10, 20, 30, 40, 50] | 30 |
| [10, 20, 30, 40, 1000] | **30** ← el outlier no la afecta |

```mermaid
flowchart LR
    subgraph "Datos ordenados: [10, 20, 30, 40, 1000]"
        A["10"] --- B["20"] --- C["🎯 Mediana = 30"] --- D["40"] --- E["1000"]
    end
    
    style C fill:#e74c3c,color:#fff
```

#### Moda

La moda es el valor que aparece con mayor frecuencia. Única medida de tendencia central para datos categóricos.

```python
df["columna"].mode()[0]
```

| Datos | Moda |
|-------|------|
| ["rojo", "azul", "rojo", "verde", "rojo"] | "rojo" |
| [1, 1, 2, 2, 3] | 1 y 2 (bimodal) |

#### Media vs Mediana vs Moda — Visualmente

```mermaid
flowchart LR
    subgraph "Distribución Simétrica"
        A1["Media = Mediana = Moda"]
    end
    
    subgraph "Sesgo Positivo (derecha)"
        A2["Moda < Mediana < Media"]
    end
    
    subgraph "Sesgo Negativo (izquierda)"
        A3["Media < Mediana < Moda"]
    end
    
    style A1 fill:#2ecc71,color:#fff
    style A2 fill:#e67e22,color:#fff
    style A3 fill:#3498db,color:#fff
```

---

### Dispersión

La **dispersión** mide qué tan **alejados** están los datos entre sí o del centro.

#### Rango

Es la medida más simple: **valor máximo − valor mínimo**.

```
Rango = max(datos) - min(datos)
```

```python
df["columna"].max() - df["columna"].min()
```

| Conjunto | Datos | Rango |
|----------|-------|-------|
| A | [10, 20, 30, 40, 50] | 40 |
| B | [10, 10, 10, 50, 50] | 40 (mismo rango, pero distribución distinta) |

> ⚠️ El rango solo usa **2 valores** (mínimo y máximo), ignora todo lo demás.

#### Varianza

Mide qué tan **dispersos** están los datos respecto a la media. Una varianza alta = datos muy dispersos; varianza baja = datos concentrados alrededor de la media.

```
Varianza = Σ(xᵢ - media)² / n
```

```python
df["columna"].var()
```

```mermaid
flowchart LR
    subgraph "Varianza baja"
        A["📊 Datos cerca de la media<br/>[45, 48, 50, 52, 55]"]
    end
    
    subgraph "Varianza alta"
        B["📊 Datos lejos de la media<br/>[10, 30, 50, 70, 90]"]
    end
    
    style A fill:#2ecc71,color:#fff
    style B fill:#e74c3c,color:#fff
```

#### Desviación Estándar

Es la **raíz cuadrada de la varianza**. Se interpreta en las mismas unidades que los datos originales, lo que la hace más fácil de entender.

```
Desv. Estándar = √Varianza
```

```python
df["columna"].std()
```

| Conjunto | Datos | Media | Desviación Estándar |
|----------|-------|-------|---------------------|
| A | [45, 48, 50, 52, 55] | 50 | ≈ **3.7** |
| B | [10, 30, 50, 70, 90] | 50 | ≈ **28.3** |

**Regla empírica (distribución normal):**

```mermaid
flowchart TB
    subgraph "En una distribución normal"
        A["68% de datos<br/>están a ±1σ de la media"]
        B["95% de datos<br/>están a ±2σ de la media"]
        C["99.7% de datos<br/>están a ±3σ de la media"]
    end
    
    style A fill:#3498db,color:#fff
    style B fill:#2980b9,color:#fff
    style C fill:#1abc9c,color:#fff
```

---

### Forma de distribución

#### Skewness (Sesgo / Asimetría)

Mide qué tan **asimétrica** es la distribución de los datos.

```python
df["columna"].skew()
```

```mermaid
flowchart LR
    subgraph "Sesgo = 0<br/>Simétrica"
        A["🔵 Normal"]
    end
    
    subgraph "Sesgo > 0<br/>Cola a la derecha"
        B["🔴 Sesgo positivo<br/>Ej: ingresos<br/>(pocos ganan mucho)"]
    end
    
    subgraph "Sesgo < 0<br/>Cola a la izquierda"
        C["🟢 Sesgo negativo<br/>Ej: edad de jubilación<br/>(muchos se jubilan jóvenes)"]
    end
    
    style A fill:#3498db,color:#fff
    style B fill:#e74c3c,color:#fff
    style C fill:#2ecc71,color:#fff
```

#### Kurtosis (Curtosis)

Mide qué tan **"afilada"** o **"aplanada"** es la distribución respecto a una normal.

```python
df["columna"].kurtosis()
```

| Valor | Significado | Forma |
|-------|-------------|-------|
| **Kurtosis = 0** | Normal (mesocúrtica) | 📊 Normal |
| **Kurtosis > 0** | Más puntiaguda (leptocúrtica) | 📈 Pico alto, colas gruesas |
| **Kurtosis < 0** | Más aplanada (platicúrtica) | 📉 Pico bajo, colas delgadas |

---

### Tabla resumen: todas las medidas

| Categoría | Medida | ¿Qué mide? | Python | R |
|-----------|--------|-----------|--------|----|
| **Central** | Media | Promedio aritmético | `df['x'].mean()` | `mean(df$x)` |
| **Central** | Mediana | Valor central | `df['x'].median()` | `median(df$x)` |
| **Central** | Moda | Valor más frecuente | `df['x'].mode()[0]` | `names(sort(-table(df$x)))[1]` |
| **Dispersión** | Rango | Máx − mín | `df['x'].max() - df['x'].min()` | `max(df$x) - min(df$x)` |
| **Dispersión** | Varianza | Dispersión cuadrada | `df['x'].var()` | `var(df$x)` |
| **Dispersión** | Desv. Estándar | Dispersión en unidades originales | `df['x'].std()` | `sd(df$x)` |
| **Forma** | Skewness | Asimetría | `df['x'].skew()` | `library(e1071); skewness(df$x)` |
| **Forma** | Kurtosis | Punta / aplanamiento | `df['x'].kurtosis()` | `library(e1071); kurtosis(df$x)` |

### Código completo: estadísticas descriptivas

```python
import pandas as pd

df = pd.read_csv("datos.csv")

# Todas las estadísticas descriptivas de un tirón
df.describe()  # Media, std, min, quartiles, max

# Por columna
print(f"Media:      {df['edad'].mean():.2f}")
print(f"Mediana:    {df['edad'].median():.2f}")
print(f"Moda:       {df['edad'].mode()[0]}")
print(f"Rango:      {df['edad'].max() - df['edad'].min()}")
print(f"Varianza:   {df['edad'].var():.2f}")
print(f"Desv. Std:  {df['edad'].std():.2f}")
print(f"Sesgo:      {df['edad'].skew():.2f}")
print(f"Curtosis:   {df['edad'].kurtosis():.2f}")
```

---

## Visualizando distribuciones

Los números son útiles, pero **una imagen vale más que mil estadísticas**. Las visualizaciones te permiten ver patrones que los números no muestran.

```mermaid
flowchart TD
    Q["🎯 ¿Qué quieres visualizar?"]
    
    Q --> V1["Distribución de<br/>1 variable numérica"]
    V1 --> G1["Histograma o Box Plot"]
    
    Q --> V2["Distribución de<br/>1 variable categórica"]
    V2 --> G2["Gráfico de Barras"]
    
    Q --> V3["Relación entre<br/>2 variables numéricas"]
    V3 --> G3["Scatter Plot"]
    
    Q --> V4["Comparar distribución<br/>entre categorías"]
    V4 --> G4["Box Plot agrupado"]
    
    Q --> V5["Todas las relaciones<br/>(exploratorio)"]
    V5 --> G5["Pairplot / Correlación"]
    
    style Q fill:#3498db,color:#fff
    style G1 fill:#2ecc71,color:#fff
    style G2 fill:#2ecc71,color:#fff
    style G3 fill:#2ecc71,color:#fff
    style G4 fill:#2ecc71,color:#fff
    style G5 fill:#2ecc71,color:#fff
```

### Histograma

Muestra la **frecuencia** de valores en intervalos (bins). Ideal para entender la forma de la distribución.

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Histograma básico
df["edad"].hist(bins=30, edgecolor="black")
plt.title("Distribución de Edad")
plt.xlabel("Edad")
plt.ylabel("Frecuencia")
plt.show()

# Histograma con seaborn (más bonito)
sns.histplot(data=df, x="edad", bins=30, kde=True)  # kde=True añade curva
plt.title("Distribución de Edad con KDE")
plt.show()
```

```r
library(ggplot2)

# Histograma
ggplot(df, aes(x = edad)) +
  geom_histogram(bins = 30, fill = "steelblue", color = "black") +
  labs(title = "Distribución de Edad")

# Histograma + curva de densidad
ggplot(df, aes(x = edad)) +
  geom_histogram(aes(y = after_stat(density)), bins = 30, fill = "steelblue") +
  geom_density(color = "red", linewidth = 1) +
  labs(title = "Distribución de Edad con KDE")
```

### Box Plot (Diagrama de Caja y Bigotes)

Resume **5 estadísticas** de un vistazo: mínimo, Q1, mediana, Q3, máximo (y muestra outliers).

```mermaid
flowchart LR
    subgraph "Box Plot"
        A["⛔ Outlier"] --- B["Límite inf."]
        B --- C["Q1 (25%)"]
        C --- D["Mediana (50%)"]
        D --- E["Q3 (75%)"]
        E --- F["Límite sup."]
        F --- G["⛔ Outlier"]
    end
    
    style A fill:#e74c3c,color:#fff
    style G fill:#e74c3c,color:#fff
```

```python
# Box plot simple
sns.boxplot(data=df, x="columna")
plt.title("Box Plot")
plt.show()

# Box plot por categoría (comparar grupos)
sns.boxplot(data=df, x="categoria", y="valor")
plt.title("Distribución por Categoría")
plt.show()
```

```r
# Box plot simple
ggplot(df, aes(y = columna)) +
  geom_boxplot(fill = "steelblue") +
  labs(title = "Box Plot")

# Box plot por categoría
ggplot(df, aes(x = categoria, y = valor)) +
  geom_boxplot(fill = "steelblue") +
  labs(title = "Distribución por Categoría")
```

### Densidad (KDE)

Una versión **suavizada** del histograma. Muestra la forma de la distribución sin depender del número de bins.

```python
sns.kdeplot(data=df, x="edad", fill=True)
plt.title("Densidad de Edad")
plt.show()
```

### Múltiples distribuciones en un solo gráfico

```python
# Comparar distribuciones por categoría
sns.kdeplot(data=df, x="edad", hue="genero", fill=True)
plt.title("Distribución de Edad por Género")
plt.show()
```

### Matriz de correlación

Muestra cómo se **relacionan** todas las variables numéricas entre sí.

```python
import seaborn as sns

# Calcular correlaciones
corr = df.corr()

# Heatmap de correlaciones
sns.heatmap(corr, annot=True, cmap="coolwarm", center=0)
plt.title("Matriz de Correlación")
plt.show()
```

### Pairplot (todas las relaciones)

El gráfico exploratorio definitivo: muestra **todas las combinaciones** de variables.

```python
# Pairplot (solo numéricas)
sns.pairplot(df, hue="categoria")
plt.show()
```

---

## Flujo completo: análisis descriptivo

```mermaid
flowchart TD
    START["📥 Datos"] --> EXPLORE["🔍 Exploración inicial<br/>head, info, describe"]
    EXPLORE --> CENTRAL["📌 Tendencia Central<br/>Media, Mediana, Moda"]
    CENTRAL --> DISP["📏 Dispersión<br/>Rango, Varianza, Std"]
    DISP --> FORM["📈 Forma<br/>Skewness, Kurtosis"]
    FORM --> VIZ["📊 Visualizar<br/>Histograma, Box Plot, Pairplot"]
    VIZ --> INSIGHTS["💡 Insights<br/>Patrones, anomalías, hipótesis"]
    
    style START fill:#3498db,color:#fff
    style EXPLORE fill:#f39c12,color:#000
    style CENTRAL fill:#e74c3c,color:#fff
    style DISP fill:#9b59b6,color:#fff
    style FORM fill:#1abc9c,color:#fff
    style VIZ fill:#2ecc71,color:#fff
    style INSIGHTS fill:#e67e22,color:#fff
```

> 📁 **Ver también**: [[introduccion-analisis-de-datos]], [[limpieza-de-datos]], [[ganar-habilidades-de-programacion]]

## Relacionados:
- [[limpieza-de-datos]] #anterior 
- [[visualizacion-de-datos]] #siguiente 