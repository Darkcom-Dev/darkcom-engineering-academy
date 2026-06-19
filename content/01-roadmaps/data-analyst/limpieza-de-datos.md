# Limpieza de datos

La limpieza de datos (también llamada **data cleaning** o **data wrangling**) es el proceso de **detectar y corregir** errores, inconsistencias y valores atípicos en los datos. Es el paso que consume **~80% del tiempo** de un analista, pero es **fundamental** para obtener resultados confiables.

```mermaid
flowchart TB
    subgraph "Proporción del tiempo en análisis"
        A["🧹 Limpieza<br/>80%"]
        B["📐 Análisis<br/>15%"]
        C["📊 Reporte<br/>5%"]
    end
    
    style A fill:#e74c3c,color:#fff
    style B fill:#3498db,color:#fff
    style C fill:#2ecc71,color:#fff
```

---

## Problemas comunes en los datos

```mermaid
mindmap
  root((Problemas de<br/>calidad))
    Valores faltantes
      NaN / NULL
      Celdas vacías
      "N/A", "-", "?"
    Duplicados
      Filas repetidas
      IDs duplicados
    Outliers
      Errores de medición
      Valores extremos reales
    Inconsistencias
      "USA" vs "EE.UU."
      "1000" vs "1,000"
      Fechas en formatos distintos
    Errores de tipo
      Números como texto
      Fechas como string
```

---

## Manejo de datos perdidos

Los **valores faltantes** son uno de los problemas más comunes. En Python aparecen como `NaN` (Not a Number) o `None`; en R como `NA`.

### ¿Por qué faltan datos?

```mermaid
flowchart LR
    MISS["❓ Dato faltante"]
    
    MISS --> MCAR["Totalmente al azar<br/>(MCAR)"]
    MISS --> MAR["Al azar condicionado<br/>(MAR)"]
    MISS --> MNAR["No al azar<br/>(MNAR)"]
    
    MCAR --> EX1["Ej: El sensor falló<br/>aleatoriamente"]
    MAR --> EX2["Ej: Mujeres no responden<br/>pregunta de ingresos"]
    MNAR --> EX3["Ej: Personas con ingresos<br/>altos no reportan"]
    
    style MISS fill:#e74c3c,color:#fff
```

### Cómo identificar valores faltantes

```python
import pandas as pd

# Detectar valores faltantes
df.isnull().sum()         # Conteo por columna
df.isnull().sum().sum()   # Total de faltantes
df.isnull().mean() * 100  # Porcentaje por columna

# Visualizar patrón de faltantes
import missingno as msno
msno.matrix(df)           # Matriz de valores faltantes
msno.heatmap(df)          # Correlación entre faltantes
```

```r
library(dplyr)

# Detectar valores faltantes
sapply(df, function(x) sum(is.na(x)))  # Conteo por columna
sum(is.na(df))                          # Total de faltantes
colMeans(is.na(df)) * 100              # Porcentaje por columna
```

### Estrategias para manejar datos perdidos

```mermaid
flowchart TD
    Q["📊 ¿Cómo manejar<br/>valores faltantes?"]
    
    Q --> S1["Eliminar filas<br/>(drop)"]
    Q --> S2["Eliminar columna"]
    Q --> S3["Imputar valores"]
    Q --> S4["Mantener (modelos<br/>que toleran NaN)"]
    
    S1 --> S1A["Usar si:<br/>- Pocos faltantes<br/>- Son aleatorios<br/>- Muestra grande"]
    S2 --> S2A["Usar si:<br/>- Columna >50% faltante<br/>- Bajo valor predictivo"]
    S3 --> S3A["Usar si:<br/>- Faltantes moderados<br/>- No quieres perder datos"]
    S4 --> S4A["Usar si:<br/>- Modelo lo soporta<br/>(XGBoost, LightGBM)"]
    
    style Q fill:#3498db,color:#fff
    style S1 fill:#e74c3c,color:#fff
    style S2 fill:#e74c3c,color:#fff
    style S3 fill:#f39c12,color:#000
    style S4 fill:#2ecc71,color:#fff
```

#### 1. Eliminar filas con valores faltantes

```python
# Eliminar cualquier fila con al menos un NaN
df_clean = df.dropna()

# Eliminar si cierta columna tiene NaN
df_clean = df.dropna(subset=["columna_importante"])
```

```r
# Eliminar cualquier fila con al menos un NA
df_clean <- na.omit(df)

# Eliminar si cierta columna tiene NA
df_clean <- df[!is.na(df$columna_importante), ]
```

#### 2. Imputar (rellenar) valores faltantes

| Estrategia | Cuándo usarla | Código |
|-----------|--------------|--------|
| **Media / Mediana** | Variables numéricas con distribución normal | `df.fillna(df.mean())` |
| **Moda** | Variables categóricas | `df.fillna(df.mode()[0])` |
| **Valor constante** | Cuando sabes el valor por defecto | `df.fillna(0)` |
| **Forward fill** | Series de tiempo (usar valor anterior) | `df.fillna(method="ffill")` |
| **Backward fill** | Series de tiempo (usar valor siguiente) | `df.fillna(method="bfill")` |
| **Interpolación** | Series de tiempo (estimar entre puntos) | `df.interpolate()` |
| **KNN / Modelo** | Cuando hay relación entre variables | `from sklearn.impute import KNNImputer` |

```python
# Imputación en Python
df["edad"] = df["edad"].fillna(df["edad"].median())     # Mediana
df["genero"] = df["genero"].fillna(df["genero"].mode()[0])  # Moda
df["ingreso"] = df["ingreso"].fillna(0)                  # Constante
df["temperatura"] = df["temperatura"].interpolate()       # Interpolación
```

```r
# Imputación en R
library(tidyr)

df$edad <- ifelse(is.na(df$edad), median(df$edad, na.rm = TRUE), df$edad)
df <- df %>% fill(temperatura, .direction = "down")  # Forward fill
```

> ⚠️ **Precaución**: Imputar puede introducir sesgo. Siempre documenta cómo manejaste los valores faltantes.

---

## Removiendo duplicados

Los **duplicados** son registros idénticos (o con el mismo identificador) que pueden distorsionar tus análisis.

### Cómo identificar duplicados

```python
# Encontrar duplicados
df.duplicated()                    # Booleanos (True = duplicado)
df.duplicated().sum()              # Total de duplicados
df[df.duplicated()]                # Ver filas duplicadas

# Duplicados basados en columnas específicas
df.duplicated(subset=["email", "id"])
```

```r
# Encontrar duplicados
sum(duplicated(df))               # Total de duplicados
df[duplicated(df), ]              # Ver filas duplicadas

# Duplicados basados en columnas específicas
df[duplicated(df[c("email", "id")]), ]
```

### Cómo eliminar duplicados

```python
# Eliminar duplicados (conserva la primera ocurrencia)
df_sin_dups = df.drop_duplicates()

# Conservar la última ocurrencia
df_sin_dups = df.drop_duplicates(keep="last")

# Basado en columnas específicas
df_sin_dups = df.drop_duplicates(subset=["email"])

# Reiniciar índice después de limpiar
df_sin_dups = df.drop_duplicates().reset_index(drop=True)
```

```r
# Eliminar duplicados
library(dplyr)

df_sin_dups <- distinct(df)                      # Todas las columnas
df_sin_dups <- distinct(df, email, .keep_all = TRUE)  # Solo email
```

```mermaid
flowchart LR
    A["📋 Datos originales<br/>100 filas"] --> B["🔍 Detectar duplicados"]
    B --> C["❌ 2 filas duplicadas<br/>encontradas"]
    C --> D["✅ Datos limpios<br/>98 filas"]
    
    style A fill:#3498db,color:#fff
    style B fill:#f39c12,color:#000
    style C fill:#e74c3c,color:#fff
    style D fill:#2ecc71,color:#fff
```

---

## Encontrando Outliers

Los **outliers** (valores atípicos) son observaciones que se alejan significativamente del resto. Pueden ser errores o información valiosa.

> 💡 **Regla práctica**: No todos los outliers son errores. Un ingreso de $1M puede ser un outlier legítimo (una persona rica). El contexto lo determina.

### Métodos para detectar outliers

```mermaid
flowchart TD
    Q["🔍 Métodos para detectar outliers"]
    
    Q --> M1["Rango Intercuartil<br/>(IQR)"]
    Q --> M2["Z-Score"]
    Q --> M3["Visualización"]
    
    M1 --> M1A["Valores fuera de:<br/>Q1 - 1.5×IQR<br/>Q3 + 1.5×IQR"]
    M2 --> M2A["Valores con |Z| > 3<br/>(a más de 3 desviaciones<br/>de la media)"]
    M3 --> M3A["Box plots<br/>Scatter plots<br/>Histogramas"]
    
    style Q fill:#3498db,color:#fff
    style M1 fill:#e67e22,color:#fff
    style M2 fill:#9b59b6,color:#fff
    style M3 fill:#1abc9c,color:#fff
```

#### Método 1: IQR (Rango Intercuartil)

```python
# Método IQR para detectar outliers
Q1 = df["columna"].quantile(0.25)
Q3 = df["columna"].quantile(0.75)
IQR = Q3 - Q1

limite_inferior = Q1 - 1.5 * IQR
limite_superior = Q3 + 1.5 * IQR

outliers = df[(df["columna"] < limite_inferior) | (df["columna"] > limite_superior)]
print(f"Outliers encontrados: {len(outliers)}")
```

```r
# Método IQR en R
Q1 <- quantile(df$columna, 0.25)
Q3 <- quantile(df$columna, 0.75)
IQR <- Q3 - Q1

limite_inferior <- Q1 - 1.5 * IQR
limite_superior <- Q3 + 1.5 * IQR

outliers <- df[df$columna < limite_inferior | df$columna > limite_superior, ]
print(paste("Outliers encontrados:", nrow(outliers)))
```

```mermaid
flowchart LR
    subgraph "Box Plot - IQR"
        A["⛔ Outliers"] --- B["Límite inferior<br/>Q1 - 1.5×IQR"]
        B --- C["Q1"]
        C --- D["Mediana"]
        D --- E["Q3"]
        E --- F["Límite superior<br/>Q3 + 1.5×IQR"]
        F --- G["⛔ Outliers"]
    end
    
    style A fill:#e74c3c,color:#fff
    style G fill:#e74c3c,color:#fff
```

#### Método 2: Z-Score

```python
from scipy import stats

# Calcular Z-Score
z_scores = stats.zscore(df["columna"])
outliers = df[abs(z_scores) > 3]
print(f"Outliers (|Z| > 3): {len(outliers)}")
```

#### Método 3: Visualización

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Box plot
sns.boxplot(data=df, x="columna")
plt.show()

# Histograma
df["columna"].hist(bins=50)
plt.show()

# Scatter plot (para ver relación + outliers)
sns.scatterplot(data=df, x="col1", y="col2")
plt.show()
```

### ¿Qué hacer con los outliers?

```mermaid
flowchart TD
    Q["🔍 Encontraste un outlier"]
    
    Q --> DEC{"¿Es un error<br/>o es real?"}
    
    DEC -->|"❌ Error<br/>(medición, captura)"| REMOVE["Eliminar o corregir"]
    DEC -->|"✅ Real<br/>(valor genuino)"| KEEP["Mantener o transformar"]
    
    REMOVE --> R1["Eliminar fila"]
    REMOVE --> R2["Corregir valor"]
    REMOVE --> R3["Imputar con media/mediana"]
    
    KEEP --> K1["Mantener (si el modelo es robusto)"]
    KEEP --> K2["Transformación logarítmica"]
    KEEP --> K3["Capping (limitar a percentiles 1-99)"]
    
    style Q fill:#3498db,color:#fff
    style REMOVE fill:#e74c3c,color:#fff
    style KEEP fill:#2ecc71,color:#fff
```

---

## Data transformation (Transformación de datos)

A veces los datos no están en el formato que necesitas para analizarlos. Aquí es donde entran las **transformaciones**.

### Tipos de transformaciones comunes

```mermaid
flowchart TD
    T["🔧 Transformaciones"]
    
    T --> T1["Cambio de tipo<br/>(string → número, texto → fecha)"]
    T --> T2["Escalamiento / Normalización"]
    T --> T3["Codificación de categóricas"]
    T --> T4["Creación de nuevas<br/>columnas (feature engineering)"]
    
    T1 --> EX1["df['precio'] = df['precio'].astype(float)"]
    T2 --> EX2["MinMaxScaler, StandardScaler"]
    T3 --> EX3["One-Hot Encoding, Label Encoding"]
    T4 --> EX4["Edad = fecha_actual - fecha_nacimiento"]
    
    style T fill:#3498db,color:#fff
```

#### 1. Cambio de tipos

```python
# Convertir tipos en Python
df["precio"] = df["precio"].astype(float)                    # String → float
df["fecha"] = pd.to_datetime(df["fecha"])                    # String → datetime
df["codigo"] = df["codigo"].astype(str)                      # Número → string
df["activo"] = df["activo"].map({"Sí": True, "No": False})  # Categórica → booleano
```

```r
# Convertir tipos en R
df$precio <- as.numeric(df$precio)                           # String → numérico
df$fecha <- as.Date(df$fecha)                                # String → Date
df$codigo <- as.character(df$codigo)                         # Número → string
df$activo <- ifelse(df$activo == "Sí", TRUE, FALSE)          # Categórica → booleano
```

#### 2. Escalamiento / Normalización

```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler

# Estandarización (media=0, desv=1) — ideal para ML
scaler = StandardScaler()
df["edad_std"] = scaler.fit_transform(df[["edad"]])

# Normalización (0 a 1)
scaler = MinMaxScaler()
df["edad_norm"] = scaler.fit_transform(df[["edad"]])
```

#### 3. Codificación de variables categóricas

```python
# One-Hot Encoding (crear columnas dummy)
df_encoded = pd.get_dummies(df, columns=["region"], drop_first=True)

# Label Encoding (asignar número a cada categoría)
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
df["region_cod"] = le.fit_transform(df["region"])
```

| Categoría original | One-Hot Encoding (3 columnas) | Label Encoding |
|-------------------|-------------------------------|----------------|
| Norte | Norte=1, Sur=0, Este=0 | 0 |
| Sur | Norte=0, Sur=1, Este=0 | 1 |
| Este | Norte=0, Sur=0, Este=1 | 2 |

#### 4. Feature Engineering (crear columnas)

```python
# Ejemplos de nuevas columnas
df["edad"] = 2025 - df["año_nacimiento"]            # Cálculo simple
df["ingreso_anual"] = df["ingreso_mensual"] * 12     # Escalamiento
df["nombre_completo"] = df["nombre"] + " " + df["apellido"]    # Concatenación
df["fecha_trimestre"] = df["fecha"].dt.to_period("Q")          # Extraer trimestre
df["es_fin_semana"] = df["fecha"].dt.weekday >= 5              # Extraer día
```

---

## Usando librerías para limpieza

### Pandas (Python)

Pandas es la librería más importante para manipulación y limpieza de datos en Python.

```python
import pandas as pd

# === 1. CARGAR ===
df = pd.read_csv("datos.csv")

# === 2. EXPLORAR ===
df.head()                 # Primeras filas
df.info()                 # Tipos de datos y no nulos
df.describe()             # Estadísticas descriptivas
df.shape                  # Dimensiones (filas, columnas)
df.columns                # Nombres de columnas

# === 3. LIMPIAR ===

# Renombrar columnas
df.columns = df.columns.str.lower().str.replace(" ", "_")
df.rename(columns={"ventas_2024": "ventas"}, inplace=True)

# Valores faltantes
df.isnull().sum()
df.dropna(subset=["columna_critica"], inplace=True)
df["edad"].fillna(df["edad"].median(), inplace=True)

# Duplicados
df.drop_duplicates(inplace=True)

# Outliers (IQR)
Q1, Q3 = df["precio"].quantile([0.25, 0.75])
IQR = Q3 - Q1
df = df[(df["precio"] >= Q1 - 1.5*IQR) & (df["precio"] <= Q3 + 1.5*IQR)]

# Cambio de tipos
df["fecha"] = pd.to_datetime(df["fecha"])
df["precio"] = pd.to_numeric(df["precio"], errors="coerce")

# === 4. GUARDAR ===
df.to_csv("datos_limpios.csv", index=False)
```

### Dplyr (R)

Dplyr es parte del **tidyverse** y ofrece una sintaxis limpia y consistente para manipular datos.

```r
library(dplyr)
library(tidyr)

# === 1. CARGAR ===
df <- read_csv("datos.csv")

# === 2. EXPLORAR ===
glimpse(df)               # Vista previa de estructura
summary(df)               # Estadísticas descriptivas
dim(df)                   # Dimensiones
names(df)                 # Nombres de columnas

# === 3. LIMPIAR ===

# Renombrar columnas
df <- df %>%
  rename_with(tolower) %>%
  rename_with(~ gsub(" ", "_", .x))

# Valores faltantes
colSums(is.na(df))
df <- df %>%
  drop_na(columna_critica) %>%
  mutate(edad = ifelse(is.na(edad), median(edad, na.rm = TRUE), edad))

# Duplicados
df <- df %>% distinct()

# Outliers (IQR)
Q1 <- quantile(df$precio, 0.25)
Q3 <- quantile(df$precio, 0.75)
IQR <- Q3 - Q1
df <- df %>%
  filter(precio >= Q1 - 1.5*IQR & precio <= Q3 + 1.5*IQR)

# Cambio de tipos
df <- df %>%
  mutate(
    fecha = as.Date(fecha),
    precio = as.numeric(precio)
  )

# === 4. GUARDAR ===
write_csv(df, "datos_limpios.csv")
```

### Flujo completo de limpieza

```mermaid
flowchart TD
    START["📥 Datos crudos"] --> EXPLORE["🔍 Explorar<br/>head, info, describe"]
    EXPLORE --> MISSING["🧩 Valores faltantes<br/>isnull → fillna / dropna"]
    MISSING --> DUPS["🔁 Duplicados<br/>duplicated → drop_duplicates"]
    DUPS --> OUTLIERS["📊 Outliers<br/>IQR / Z-Score → filtrar"]
    OUTLIERS --> TYPES["🔧 Tipos de datos<br/>astype, to_datetime"]
    TYPES --> FEATURES["✨ Feature Engineering<br/>nuevas columnas"]
    FEATURES --> SAVE["💾 Datos limpios<br/>to_csv / write_csv"]
    
    style START fill:#ff6b6b,color:#fff
    style EXPLORE fill:#3498db,color:#fff
    style MISSING fill:#e74c3c,color:#fff
    style DUPS fill:#f39c12,color:#000
    style OUTLIERS fill:#9b59b6,color:#fff
    style TYPES fill:#1abc9c,color:#fff
    style FEATURES fill:#e67e22,color:#fff
    style SAVE fill:#2ecc71,color:#fff
```

---

## Buenas prácticas

| Práctica | Descripción |
|----------|-------------|
| **Respaldar datos originales** | Nunca modifiques los datos crudos. Trabaja siempre sobre una copia |
| **Documentar cambios** | Lleva un registro de cada transformación que aplicaste |
| **Validar después de limpiar** | Verifica que las dimensiones, tipos y rangos sean correctos |
| **Crear pipeline reproducible** | Escribe un script que ejecute toda la limpieza de principio a fin |
| **No imputar sin pensar** | Entiende por qué faltan datos antes de decidir cómo manejarlos |
| **Visualizar antes y después** | Un box plot antes/después de limpiar outliers muestra tu impacto |

> 📁 **Ver también**: [[recoleccion-de-datos]], [[ganar-habilidades-de-programacion]], [[analisis-descriptivo]]

## Relacionados
- [[recoleccion-de-datos]] #anterior 
- [[analisis-descriptivo]] #siguiente 