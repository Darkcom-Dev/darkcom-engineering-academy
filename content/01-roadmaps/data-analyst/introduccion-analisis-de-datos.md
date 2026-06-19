# Introducción al Análisis de Datos

El análisis de datos es el proceso de **transformar datos crudos en información útil** para la toma de decisiones. Imagina que los datos son como piezas de un rompecabezas: el análisis es el proceso de armarlas para ver la imagen completa.

```mermaid
flowchart LR
    A[Datos Crudos] --> B[Procesamiento]
    B --> C[Información]
    C --> D[Decisiones]
    
    style A fill:#ff6b6b,color:#fff
    style B fill:#feca57,color:#000
    style C fill:#48dbfb,color:#000
    style D fill:#1dd1a1,color:#fff
```

---

## ¿Qué es un analista de datos?

Un **analista de datos** es un profesional que recolecta, procesa y analiza datos para ayudar a las organizaciones a tomar mejores decisiones. Es un **detective de información**: busca patrones, encuentra pistas y resuelve problemas usando datos.

### Habilidades clave de un analista de datos

```mermaid
mindmap
  root((Analista de Datos))
    Técnicas
      SQL
      Python / R
      Excel
      Estadística
    Analíticas
      Pensamiento crítico
      Resolución de problemas
      Atención al detalle
    Blandas
      Comunicación
      Trabajo en equipo
      Curiosidad
    Herramientas
      Tableau / Power BI
      Pandas / Dplyr
      Jupyter Notebooks
```

### Día a día de un analista

1. **Reunirse con stakeholders** para entender qué necesitan saber
2. **Recolectar datos** de bases de datos, APIs o archivos
3. **Limpiar y preparar** los datos para el análisis
4. **Explorar y analizar** usando estadística y visualizaciones
5. **Comunicar hallazgos** mediante informes, dashboards o presentaciones
6. **Hacer recomendaciones** basadas en los datos

---

## Tipos de análisis de datos

Existen **4 tipos principales** de análisis, ordenados de menor a mayor complejidad y valor:

```mermaid
flowchart TB
    subgraph " "
        direction TB
        A1["📊 Descriptivo<br/><i>¿Qué pasó?</i>"]
        A2["🔍 Diagnóstico<br/><i>¿Por qué pasó?</i>"]
        A3["🔮 Predictivo<br/><i>¿Qué pasará?</i>"]
        A4["💡 Prescriptivo<br/><i>¿Qué debemos hacer?</i>"]
    end

    A1 --> A2 --> A3 --> A4

    style A1 fill:#3498db,color:#fff
    style A2 fill:#9b59b6,color:#fff
    style A3 fill:#e67e22,color:#fff
    style A4 fill:#e74c3c,color:#fff
```

### Análisis descriptivo

> **"¿Qué pasó?"**

Es el tipo más básico y común. Responde **qué ocurrió** en el pasado usando datos históricos.

- **Ejemplo**: "Las ventas del Q1 2025 fueron de $1.2M, un 15% más que el Q1 2024"
- **Técnicas**: Promedios, totales, porcentajes, gráficos de barras/líneas
- **Herramientas**: Excel, SQL, Tableau, Power BI

```mermaid
graph LR
    subgraph "Análisis Descriptivo"
        B[Datos Históricos] --> C[Métricas Resumen]
        C --> D[Reporte / Dashboard]
    end
    D --> E["📈 Insight: <br/>'Las ventas subieron 15%'"]
    
    style B fill:#3498db,color:#fff
    style C fill:#3498db,color:#fff
    style D fill:#3498db,color:#fff
    style E fill:#2ecc71,color:#fff
```

### Análisis diagnóstico

> **"¿Por qué pasó?"**

Va un paso más allá: busca entender **las causas** de lo que ocurrió.

- **Ejemplo**: "Las ventas subieron 15% porque lanzamos una campaña en redes sociales que generó 50,000 visitas nuevas"
- **Técnicas**: Segmentación, correlación, drill-down, análisis de causa raíz
- **Herramientas**: SQL (GROUP BY, JOINs), Python (correlaciones), Excel (tablas dinámicas)

```mermaid
graph TD
    A["❓ ¿Por qué subieron las ventas?"] --> B[Analizar campañas]
    A --> C[Analizar temporada]
    A --> D[Analizar competencia]
    
    B --> E["Campaña en redes<br/>nuevas 50,000 visitas"]
    C --> F["Sin efecto estacional<br/>significativo"]
    D --> G["Competidor principal<br/>sin cambios"]
    
    E --> H["✅ Causa raíz identificada:<br/>Campaña en redes sociales"]
    
    style A fill:#9b59b6,color:#fff
    style H fill:#2ecc71,color:#fff
```

### Análisis predictivo

> **"¿Qué pasará?"**

Usa datos históricos + modelos estadísticos para **pronosticar** el futuro.

- **Ejemplo**: "Basado en las tendencias actuales, las ventas del Q2 2025 serán de aproximadamente $1.4M"
- **Técnicas**: Regresión lineal, series de tiempo, machine learning
- **Herramientas**: Python (scikit-learn, statsmodels), R, SQL

```mermaid
graph LR
    A[Datos Históricos] --> B[Modelo Predictivo]
    B --> C[Predicción]
    
    B --> D[Regresión]
    B --> E[Series de Tiempo]
    B --> F[Redes Neuronales]
    
    style A fill:#e67e22,color:#fff
    style B fill:#e67e22,color:#fff
    style C fill:#e67e22,color:#fff
```

### Análisis prescriptivo

> **"¿Qué debemos hacer?"**

Es el más avanzado. No solo predice, sino que **recomienda acciones** específicas.

- **Ejemplo**: "Para alcanzar $1.4M en Q2, debes invertir $50K en ads y enfocarte en el segmento de 25-34 años"
- **Técnicas**: Optimización, simulación, algoritmos de recomendación, IA
- **Herramientas**: Python (PuLP, ortools), R, software especializado

```mermaid
graph TD
    A["🎯 Objetivo:<br/>Ventas Q2 = $1.4M"] --> B[Simular Escenarios]
    
    B --> C["Escenario A:<br/>Invertir $50K en ads"]
    B --> D["Escenario B:<br/>Descuentos del 10%"]
    B --> E["Escenario C:<br/>Email marketing"]
    
    C --> F["💰 Proyección: $1.42M ✓"]
    D --> G["💰 Proyección: $1.35M ✗"]
    E --> H["💰 Proyección: $1.38M ✗"]
    
    F --> I["✅ Recomendación:<br/>Ejecutar Escenario A"]
    
    style A fill:#e74c3c,color:#fff
    style I fill:#2ecc71,color:#fff
```

---

## Proceso de análisis de datos (Ciclo de vida)

El análisis de datos sigue un proceso iterativo conocido como el **ciclo de vida del dato**:

```mermaid
flowchart LR
    A["1. Recolección<br/>📥"] --> B["2. Limpieza<br/>🧹"]
    B --> C["3. Exploración<br/>🔎"]
    C --> D["4. Análisis<br/>📐"]
    D --> E["5. Visualización<br/>📊"]
    E --> F["6. Comunicación<br/>🗣️"]
    F -.->|Feedback| A
    
    style A fill:#3498db,color:#fff
    style B fill:#e74c3c,color:#fff
    style C fill:#f39c12,color:#fff
    style D fill:#9b59b6,color:#fff
    style E fill:#1abc9c,color:#fff
    style F fill:#2ecc71,color:#fff
```

---

## Conceptos clave

### Recolección

Obtener datos de diversas fuentes. Es el **primer paso** y uno de los más importantes: datos de mala calidad generan análisis incorrectos.

| Fuente | Ejemplo | Formato |
|--------|---------|---------|
| Bases de datos | PostgreSQL, MySQL | Tablas SQL |
| Archivos | CSV, Excel, JSON | Archivos planos |
| APIs | Twitter API, Stripe API | JSON / XML |
| Web Scraping | Extraer datos de sitios web | HTML / CSV |
| Sensores | IoT, dispositivos | Streaming |

> 📁 **Ver más**: [[recoleccion-de-datos]]

### Limpieza

También llamado **data wrangling** o **data cleaning**. Es el proceso de detectar y corregir errores en los datos.

**Problemas comunes:**
- **Valores faltantes** — celdas vacías (NaN, NULL)
- **Duplicados** — registros repetidos
- **Outliers** — valores extremos que distorsionan el análisis
- **Inconsistencias** — formatos distintos para el mismo dato ("USA" vs "EE.UU.")
- **Errores de tipo** — números almacenados como texto

> 📁 **Ver más**: [[limpieza-de-datos]]

### Exploración

También conocido como **EDA (Exploratory Data Analysis)**. Consiste en entender la estructura, distribución y relaciones de los datos *antes* de aplicar modelos.

```mermaid
graph TD
    A["EDA: Análisis Exploratorio"] --> B["Estructura<br/>¿Cuántas filas/columnas?"]
    A --> C["Distribuciones<br/>¿Cómo se comportan los datos?"]
    A --> D["Relaciones<br/>¿Variables correlacionadas?"]
    A --> E["Valores Atípicos<br/>¿Hay outliers?"]
    
    B --> F["df.shape, df.info()"]
    C --> G["Histogramas, Box plots"]
    D --> H["Scatter plots, Correlación"]
    E --> I["Z-score, IQR, Box plots"]
    
    style A fill:#f39c12,color:#000
```

### Visualización

Representar los datos gráficamente para identificar patrones y comunicar hallazgos de forma clara.

**Tipos de gráficos según el objetivo:**

| Gráfico | ¿Cuándo usarlo? |
|---------|-----------------|
| **Barras** | Comparar categorías |
| **Líneas** | Mostrar tendencias en el tiempo |
| **Dispersión (scatter)** | Relación entre dos variables |
| **Pastel (pie)** | Proporciones de un todo (pocas categorías) |
| **Box plot** | Distribución y outliers |
| **Heatmap** | Correlaciones o datos en matriz |
| **Histograma** | Distribución de una variable numérica |

```mermaid
graph LR
    subgraph "Librerías populares"
        A1[Python: matplotlib, seaborn, plotly]
        A2[R: ggplot2]
        A3[BI: Tableau, Power BI]
    end
    
    style A1 fill:#1abc9c,color:#fff
    style A2 fill:#1abc9c,color:#fff
    style A3 fill:#1abc9c,color:#fff
```

### Análisis estadístico

Aplicar métodos estadísticos para obtener conclusiones rigurosas de los datos.

**Dos grandes ramas:**

```mermaid
flowchart TB
    subgraph "Estadística"
        direction LR
        B["**Descriptiva**<br/>Resumir datos<br/>→ Media, mediana, desviación<br/>→ Tablas, gráficos"]
        C["**Inferencial**<br/>Sacar conclusiones<br/>→ Pruebas de hipótesis<br/>→ Intervalos de confianza"]
    end
    
    style B fill:#2980b9,color:#fff
    style C fill:#27ae60,color:#fff
```

> 📁 **Ver más**: [[analisis-descriptivo]]

### Machine Learning

Rama de la IA que permite a las computadoras **aprender patrones** de los datos sin ser programadas explícitamente para cada caso.

```mermaid
mindmap
  root((ML))
    Supervisado
      Regresión
      Clasificación
    No Supervisado
      Clustering
      Reducción de dimensionalidad
    Reforzado
      Agentes
      Recompensas
```

**Ejemplos prácticos:**
- **Supervisado**: Predecir precio de una casa (regresión) o detectar spam (clasificación)
- **No supervisado**: Segmentar clientes por comportamiento (clustering)
- **Reforzado**: Algoritmos de juegos, recomendaciones

---

## Resumen visual del flujo completo

```mermaid
flowchart TD
    START([Datos Crudos]) --> REC[Recolección]
    REC --> LIM[Limpieza]
    LIM --> EDA[Exploración / EDA]
    
    EDA --> EST["Análisis Estadístico"]
    EDA --> ML["Machine Learning"]
    
    EST --> VIZ[Visualización]
    ML --> VIZ
    
    VIZ --> COM[Comunicación]
    COM --> DEC["📌 Toma de Decisiones"]
    
    START -.->|Calidad de datos| LIM
    VIZ -.->|Insights| EDA
    
    style START fill:#ff6b6b,color:#fff
    style REC fill:#3498db,color:#fff
    style LIM fill:#e74c3c,color:#fff
    style EDA fill:#f39c12,color:#000
    style EST fill:#9b59b6,color:#fff
    style ML fill:#e67e22,color:#fff
    style VIZ fill:#1abc9c,color:#fff
    style COM fill:#2ecc71,color:#fff
    style DEC fill:#1dd1a1,color:#fff
```

---

## Para empezar tu ruta de aprendizaje

```mermaid
flowchart LR
    A["1. Estadística básica<br/>📐"] --> B["2. Excel / SQL<br/>📊"]
    B --> C["3. Python / R<br/>🐍"]
    C --> D["4. Visualización<br/>📈"]
    D --> E["5. ML básico<br/>🤖"]
    E --> F["6. Portfolio<br/>📁"]
    
    style A fill:#3498db,color:#fff
    style B fill:#2980b9,color:#fff
    style C fill:#9b59b6,color:#fff
    style D fill:#1abc9c,color:#fff
    style E fill:#e67e22,color:#fff
    style F fill:#e74c3c,color:#fff
```

> 📁 **Roadmap completo**: [[data-analyst-roadmap]]

## Relacionados:
- [[analisis-reportando-con-excel]] #siguiente 