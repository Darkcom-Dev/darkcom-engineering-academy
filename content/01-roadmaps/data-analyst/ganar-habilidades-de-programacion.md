# Ganar habilidades de programación

La programación es una habilidad fundamental para el análisis de datos. Te permite **automatizar tareas**, **manipular grandes volúmenes de datos** y **crear análisis reproducibles** que Excel simplemente no puede manejar.

```mermaid
flowchart LR
    A["🤔 Sin programación<br/>Tareas manuales<br/>Límite de filas<br/>Errores humanos"] --> B["💻 Con programación<br/>Automatización<br/>Millones de filas<br/>Reproducible"]
    
    style A fill:#e74c3c,color:#fff
    style B fill:#2ecc71,color:#fff
```

---

## Aprender un lenguaje de programación

Como analista de datos, tu objetivo **no es ser ingeniero de software**, sino usar la programación como herramienta para analizar datos más rápido y mejor.

```mermaid
flowchart TD
    START["🎯 ¿Qué lenguaje elegir?"]
    START --> P["🐍 Python"]
    START --> R["📊 R"]
    
    P --> P1["✅ Más versátil<br/>✅ Machine Learning<br/>✅ Web scraping<br/>✅ Automatización"]
    R --> R1["✅ Estadística avanzada<br/>✅ Visualización elegante<br/>✅ Análisis académico<br/>✅ Shiny apps"]
    
    style START fill:#3498db,color:#fff
    style P fill:#2ecc71,color:#fff
    style R fill:#2ecc71,color:#fff
```

---

### Python

Python es el lenguaje más popular para análisis de datos por su **simplicidad, versatilidad y enorme ecosistema** de librerías.

#### ¿Por qué Python?

| Característica | Beneficio |
|---------------|-----------|
| **Sintaxis simple** | Fácil de aprender, se lee como inglés |
| **Grande comunidad** | Millones de tutoriales, foros, soluciones |
| **Multipropósito** | Análisis, ML, web, automatización |
| **Librerías maduras** | Pandas, NumPy, scikit-learn, etc. |

#### Conceptos básicos que debes dominar

```mermaid
mindmap
  root((Python para<br/>Data Analysis))
    Fundamentos
      Variables y tipos
      Listas, diccionarios
      Funciones
      Condicionales (if/else)
      Ciclos (for/while)
    Manipulación
      Pandas DataFrames
      Filtrado y selección
      GroupBy y agregaciones
      Joins (merge)
    Visualización
      matplotlib
      seaborn
      plotly
    Análisis
      Estadísticas descriptivas
      Correlaciones
      Regresión
```

#### Tu primer script en Python

```python
import pandas as pd

# Cargar datos
df = pd.read_csv("ventas.csv")

# Ver estructura
print(df.head())
print(df.info())

# Ventas totales por región
resumen = df.groupby("region")["monto"].sum()
print(resumen)
```

> **Tip**: Empieza con **Google Colab** o **Jupyter Notebook** — no necesitas instalar nada y puedes experimentar de inmediato.

#### Ruta de aprendizaje recomendada

```mermaid
flowchart LR
    A["🐍 Sintaxis básica<br/>1 semana"] --> B["📊 Pandas básico<br/>2 semanas"]
    B --> C["🧹 Limpieza de datos<br/>1 semana"]
    C --> D["📈 Visualización<br/>1 semana"]
    D --> E["📐 Estadística<br/>2 semanas"]
    E --> F["🤖 ML básico<br/>2 semanas"]
    
    style A fill:#3498db,color:#fff
    style B fill:#2980b9,color:#fff
    style C fill:#e74c3c,color:#fff
    style D fill:#1abc9c,color:#fff
    style E fill:#9b59b6,color:#fff
    style F fill:#e67e22,color:#fff
```

---

### R

R es el lenguaje preferido en **estadística académica e investigación** por su enfoque nativo en análisis de datos.

#### ¿Por qué R?

| Característica | Beneficio |
|---------------|-----------|
| **Hecho para datos** | Nace como lenguaje estadístico |
| **ggplot2** | El estándar de oro en visualización |
| **RStudio** | IDE excelente para análisis |
| **Paquetes estadísticos** | Los mejores modelos listos para usar |
| **Shiny** | Dashboards interactivos sin frontend |

#### Conceptos básicos en R

```mermaid
mindmap
  root((R para<br/>Data Analysis))
    Fundamentos
      Vectores y factores
      Data Frames
      Funciones
      tidyverse
    Manipulación
      dplyr (filter, select, mutate)
      tidyr (pivot, gather)
      Joins
    Visualización
      ggplot2
      plotly
      Shiny
    Análisis
      Modelos lineales
      Pruebas de hipótesis
      Series de tiempo
```

#### Tu primer script en R

```r
library(tidyverse)

# Cargar datos
df <- read_csv("ventas.csv")

# Ver estructura
glimpse(df)

# Ventas totales por región
resumen <- df %>%
  group_by(region) %>%
  summarise(total = sum(monto))

print(resumen)
```

#### Python vs R: ¿Cuál elegir?

```mermaid
flowchart TD
    Q["🤔 ¿Cuál es tu perfil?"]
    
    Q --> PERFIL1["📊 Ciencia de datos<br/>general / ML"]
    PERFIL1 --> REC1["🐍 Python<br/>(Recomendado)"]
    
    Q --> PERFIL2["📐 Estadística /<br/>Investigación"]
    PERFIL2 --> REC2["📊 R<br/>(Recomendado)"]
    
    Q --> PERFIL3["😅 No sé /<br/>empiezo desde cero"]
    PERFIL3 --> REC3["🐍 Python<br/>(Más versátil)"]
    
    Q --> PERFIL4["🔄 Ya sé uno,<br/>quiero el otro"]
    PERFIL4 --> REC4["¡Aprende el otro!<br/>Saber ambos es ideal"]
    
    style Q fill:#3498db,color:#fff
    style REC1 fill:#2ecc71,color:#fff
    style REC2 fill:#2ecc71,color:#fff
    style REC3 fill:#2ecc71,color:#fff
    style REC4 fill:#f39c12,color:#000
```

> 💡 **Recomendación**: Si empiezas de cero, elige **Python** por su versatilidad. Si tu trabajo es puramente estadístico, **R** es excelente. Idealmente, aprende **ambos**.

---

## Librerías de manipulación de datos

Son el **corazón** del análisis de datos programático. Te permiten cargar, limpiar, transformar y unir datos.

### Python: Pandas

Pandas introduce el **DataFrame** — una estructura de datos tabular (como una hoja de Excel) sobre la que puedes hacer operaciones poderosas con una línea de código.

```python
import pandas as pd

df = pd.read_csv("datos.csv")

# Las 4 operaciones fundamentales
df.head()              # Ver primeras filas
df.info()              # Info del DataFrame
df.describe()          # Estadísticas resumen
df.isnull().sum()      # Valores faltantes

# Selección y filtrado
df["columna"]                     # Seleccionar columna
df[df["edad"] > 30]               # Filtrar filas
df[["nombre", "edad"]]            # Seleccionar varias columnas

# Transformaciones
df["nueva_col"] = df["a"] + df["b"]  # Crear columna
df.drop_duplicates()                 # Eliminar duplicados
df.fillna(0)                         # Llenar valores nulos

# Agrupación (el GROUP BY de SQL)
df.groupby("categoria")["ventas"].sum()
df.groupby("categoria")["ventas"].agg(["sum", "mean", "count"])

# Unir tablas (JOINs)
pd.merge(df1, df2, on="id", how="left")
```

### R: dplyr + tidyr

dplyr y tidyr son parte del **tidyverse**, un ecosistema de paquetes diseñados para la manipulación de datos.

```r
library(dplyr)
library(tidyr)

df <- read_csv("datos.csv")

# Verbos principales de dplyr
select(df, columna1, columna2)    # Seleccionar columnas
filter(df, edad > 30)             # Filtrar filas
mutate(df, nueva_col = a + b)    # Crear columna
arrange(df, desc(edad))           # Ordenar
summarise(df, media = mean(edad)) # Resumir

# Agrupación
df %>%
  group_by(categoria) %>%
  summarise(
    total = sum(ventas),
    promedio = mean(ventas),
    n = n()
  )

# Joins
left_join(df1, df2, by = "id")
```

### Pandas vs dplyr: el mismo concepto en ambos lenguajes

| Operación | Python (pandas) | R (dplyr) |
|-----------|----------------|-----------|
| Filtrar | `df[df["x"] > 5]` | `filter(df, x > 5)` |
| Seleccionar | `df[["a", "b"]]` | `select(df, a, b)` |
| Crear columna | `df["c"] = df["a"] * 2` | `mutate(df, c = a * 2)` |
| Agrupar y sumar | `df.groupby("g")["v"].sum()` | `df %>% group_by(g) %>% summarise(total = sum(v))` |
| Ordenar | `df.sort_values("x")` | `arrange(df, x)` |
| Unir tablas | `pd.merge(a, b, on="id")` | `left_join(a, b, by="id")` |

```mermaid
flowchart TB
    subgraph "Manipulación de datos"
        LOAD["📥 Cargar"] --> CLEAN["🧹 Limpiar"]
        CLEAN --> TRANS["🔧 Transformar"]
        TRANS --> AGG["📊 Agrupar / Resumir"]
        AGG --> EXP["📤 Exportar"]
    end
    
    LOAD --> L1["pandas: read_csv()<br/>dplyr: read_csv()"]
    CLEAN --> C1["pandas: fillna(), drop_duplicates()<br/>dplyr: drop_na(), distinct()"]
    TRANS --> T1["pandas: df['col'] = ...<br/>dplyr: mutate()"]
    AGG --> A1["pandas: groupby().agg()<br/>dplyr: group_by() + summarise()"]
    EXP --> E1["pandas: to_csv()<br/>dplyr: write_csv()"]
    
    style LOAD fill:#3498db,color:#fff
    style CLEAN fill:#e74c3c,color:#fff
    style TRANS fill:#f39c12,color:#000
    style AGG fill:#9b59b6,color:#fff
    style EXP fill:#2ecc71,color:#fff
```

---

## Librerías de visualización de datos

Los gráficos son tu mejor herramienta para **entender los datos** y **comunicar hallazgos**.

### Python: matplotlib, seaborn y plotly

```mermaid
flowchart LR
    subgraph "Python Visualization"
        M["matplotlib<br/>🔵 Base / personalizable"] --> S["seaborn<br/>🟢 Estadístico / elegante"]
        M --> P["plotly<br/>🟠 Interactivo / web"]
        S --> P
    end
    
    style M fill:#3498db,color:#fff
    style S fill:#2ecc71,color:#fff
    style P fill:#e67e22,color:#fff
```

#### matplotlib

La librería base. Todo se construye desde cero, dándote **control total**.

```python
import matplotlib.pyplot as plt

# Gráfico de líneas simple
plt.plot(df["fecha"], df["ventas"])
plt.title("Ventas por mes")
plt.xlabel("Fecha")
plt.ylabel("Ventas ($)")
plt.show()
```

#### seaborn

Construida sobre matplotlib, ofrece gráficos **estadísticos** con mejor estilo por defecto.

```python
import seaborn as sns

# Box plot por categoría
sns.boxplot(data=df, x="categoria", y="precio")

# Heatmap de correlaciones
sns.heatmap(df.corr(), annot=True, cmap="coolwarm")

# Pairplot (todas las relaciones)
sns.pairplot(df)
```

#### plotly

Gráficos **interactivos** que funcionan en navegador. Ideal para dashboards.

```python
import plotly.express as px

# Gráfico interactivo
fig = px.scatter(df, x="ingreso", y="gasto", color="region",
                 size="poblacion", hover_data=["nombre"])
fig.show()
```

### R: ggplot2 y plotly

#### ggplot2

El **estándar de oro** de la visualización. Se basa en la **Gramática de Gráficos**: construyes el gráfico por capas.

```r
library(ggplot2)

# Box plot por categoría
ggplot(df, aes(x = categoria, y = precio)) +
  geom_boxplot() +
  labs(title = "Precio por categoría")

# Dispersión con regresión
ggplot(df, aes(x = ingreso, y = gasto, color = region)) +
  geom_point() +
  geom_smooth(method = "lm")

# Heatmap de correlaciones
df %>%
  select(where(is.numeric)) %>%
  cor() %>%
  as.data.frame() %>%
  rownames_to_column("var1") %>%
  pivot_longer(-var1, names_to = "var2", values_to = "cor") %>%
  ggplot(aes(x = var1, y = var2, fill = cor)) +
  geom_tile() +
  scale_fill_gradient2()
```

### R: Shiny — Dashboards interactivos

Shiny te permite crear **aplicaciones web interactivas** solo con R:

```r
library(shiny)

ui <- fluidPage(
  selectInput("region", "Selecciona región:", choices = unique(df$region)),
  plotOutput("grafico")
)

server <- function(input, output) {
  output$grafico <- renderPlot({
    df %>%
      filter(region == input$region) %>%
      ggplot(aes(x = mes, y = ventas)) +
      geom_line()
  })
}

shinyApp(ui, server)
```

### Comparativa: ¿qué librería usar?

```mermaid
flowchart TD
    Q["🎯 ¿Qué necesitas?"]
    
    Q --> G1["📊 Gráfico simple<br/>y rápido"]
    G1 --> A1["Python: matplotlib / seaborn<br/>R: ggplot2"]
    
    Q --> G2["🔄 Dashboard<br/>interactivo"]
    G2 --> A2["Python: plotly + Dash<br/>R: Shiny"]
    
    Q --> G3["📐 Análisis exploratorio<br/>(EDA)"]
    G3 --> A3["Python: seaborn (pairplot, heatmap)<br/>R: ggplot2 (facet_wrap)"]
    
    Q --> G4["📤 Reporte /<br/>Presentación"]
    G4 --> A4["Python: matplotlib (estático)<br/>R: ggplot2 + RMarkdown"]
    
    style Q fill:#3498db,color:#fff
    style A1 fill:#2ecc71,color:#fff
    style A2 fill:#2ecc71,color:#fff
    style A3 fill:#2ecc71,color:#fff
    style A4 fill:#2ecc71,color:#fff
```

---

## Flujo completo de trabajo: programación para análisis de datos

```mermaid
flowchart LR
    RAW["📥 Datos crudos<br/>(CSV, SQL, API)"] --> MANIP["🔧 Manipulación<br/>pandas / dplyr"]
    MANIP --> CLEAN["🧹 Limpieza<br/>fillna, drop_duplicates"]
    CLEAN --> EXPLORE["🔎 Exploración<br/>describe, groupby"]
    EXPLORE --> VIZ["📊 Visualización<br/>seaborn / ggplot2"]
    EXPLORE --> STAT["📐 Análisis<br/>estadística / ML"]
    STAT --> VIZ
    VIZ --> REPORT["📤 Reporte / Dashboard"]
    
    style RAW fill:#ff6b6b,color:#fff
    style MANIP fill:#3498db,color:#fff
    style CLEAN fill:#e74c3c,color:#fff
    style EXPLORE fill:#f39c12,color:#000
    style VIZ fill:#1abc9c,color:#fff
    style STAT fill:#9b59b6,color:#fff
    style REPORT fill:#2ecc71,color:#fff
```

---

## Recursos para aprender

| Recurso | Lenguaje | Tipo | ¿Para quién? |
|---------|----------|------|-------------|
| [Python for Everybody](https://www.py4e.com/) | Python | Curso gratuito | Principiantes |
| [R for Data Science](https://r4ds.had.co.nz/) | R | Libro gratuito | Principiantes en R |
| [Kaggle Learn](https://www.kaggle.com/learn) | Python/R | Micro-cursos interactivos | Todos los niveles |
| [DataCamp](https://www.datacamp.com/) | Python/R | Cursos interactivos | Principiantes |
| [YouTube: Corey Schafer](https://www.youtube.com/user/schafer5) | Python | Tutoriales en video | Aprendizaje visual |
| [YouTube: RStudio](https://www.youtube.com/c/RStudio) | R | Tutoriales oficiales | Usuarios de R |

> 📁 **Ver también**: [[introduccion-analisis-de-datos]], [[limpieza-de-datos]], [[data-analyst-roadmap]]

## Relacionados:
- [[analisis-reportando-con-excel]] #anterior 
- [[recoleccion-de-datos]] #siguiente 