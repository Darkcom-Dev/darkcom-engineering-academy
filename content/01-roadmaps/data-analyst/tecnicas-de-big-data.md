# Técnicas de Big Data

**Big Data** se refiere a conjuntos de datos tan **grandes, rápidos o complejos** que las herramientas tradicionales (Excel, SQL en una sola máquina, Python con pandas) no pueden procesarlos. Aquí es donde entran las técnicas y frameworks de Big Data.

> "Big Data no se trata de los datos, sino de lo que haces con ellos."

---

## Conceptos de Big Data

### Las 5 V's del Big Data

```mermaid
mindmap
  root((Big Data))
    Volumen
      Terabytes - Petabytes
      Datos que no caben<br/>en una sola máquina
    Velocidad
      Tiempo real
      Streaming
      Sensores IoT
    Variedad
      Estructurados (SQL)
      Semi-estructurados (JSON)
      No estructurados (video, texto)
    Veracidad
      Calidad de datos
      Confiabilidad
      Limpieza
    Valor
      Insights de negocio
      Decisiones basadas<br/>en datos
```

| V | Pregunta clave | Ejemplo |
|---|---------------|---------|
| **Volumen** | ¿Cuántos datos? | 10 TB de logs de servidor por día |
| **Velocidad** | ¿Qué tan rápido llegan? | 1M de tweets por minuto |
| **Variedad** | ¿Qué tipos de datos? | CSV + JSON + imágenes + video |
| **Veracidad** | ¿Son confiables? | Sensores con ruido, datos incompletos |
| **Valor** | ¿Qué ganamos? | Recomendaciones de producto personalizadas |

### Escalamiento: Vertical vs Horizontal

```mermaid
flowchart LR
    subgraph "Escalamiento Vertical"
        V["Una máquina<br/>💻 → 🖥️ → 🖥️🖥️<br/>Más RAM, más CPU<br/>Límite: hardware máximo"]
    end
    
    subgraph "Escalamiento Horizontal"
        H["Múltiples máquinas<br/>💻 💻 💻 💻 💻<br/>Trabajan en paralelo<br/>Sin límite teórico"]
    end
    
    style V fill:#3498db,color:#fff
    style H fill:#2ecc71,color:#fff
```

---

## Soluciones de almacenaje de datos

Cuando los datos no caben en una sola base de datos, necesitas soluciones distribuidas.

```mermaid
flowchart TD
    STORE["🗄️ Almacenamiento Big Data"]
    
    STORE --> DFS["Sistemas de Archivos<br/>Distribuidos"]
    STORE --> NOSQL["Bases de Datos NoSQL"]
    STORE --> LAKE["Data Lake"]
    STORE --> WH["Data Warehouse"]
    
    DFS --> DFS1["HDFS (Hadoop)"]
    DFS --> DFS2["Amazon S3"]
    
    NOSQL --> N1["MongoDB (Documentos)"]
    NOSQL --> N2["Cassandra (Columnas)"]
    NOSQL --> N3["Redis (Cache)"]
    
    LAKE --> L1["Almacén de datos crudos<br/>en su formato original<br/>AWS S3 / Azure Blob"]
    
    WH --> W1["Datos procesados<br/>y optimizados para SQL<br/>Snowflake / BigQuery"]
    
    style STORE fill:#3498db,color:#fff
    style DFS fill:#e67e22,color:#fff
    style NOSQL fill:#9b59b6,color:#fff
    style LAKE fill:#1abc9c,color:#fff
    style WH fill:#2ecc71,color:#fff
```

### Data Lake vs Data Warehouse

| Aspecto | Data Lake | Data Warehouse |
|---------|-----------|---------------|
| **Datos** | Crudos, cualquier formato | Procesados, estructurados |
| **Propósito** | Exploración, ML, almacenar | Reporting, BI, análisis |
| **Esquema** | Schema-on-read (al leer) | Schema-on-write (al escribir) |
| **Usuarios** | Data Scientists, Ingenieros | Analistas, Business users |
| **Ejemplos** | AWS S3, Azure Data Lake | Snowflake, BigQuery, Redshift |

---

## Frameworks para el procesamiento de datos

### Hadoop

Hadoop es el **framework original** de Big Data. Permite procesar grandes volúmenes de datos en un clúster de máquinas usando el modelo **MapReduce**.

```mermaid
flowchart TD
    subgraph "Arquitectura Hadoop"
        HDFS["HDFS<br/>Almacenamiento distribuido"]
        MR["MapReduce<br/>Procesamiento"]
        YARN["YARN<br/>Gestión de recursos"]
    end
    
    HDFS --> NODE1["Nodo 1<br/>Parte del archivo"]
    HDFS --> NODE2["Nodo 2<br/>Parte del archivo"]
    HDFS --> NODE3["Nodo 3<br/>Parte del archivo"]
    HDFS --> NODEN["Nodo N<br/>Parte del archivo"]
    
    style HDFS fill:#3498db,color:#fff
    style MR fill:#e74c3c,color:#fff
    style YARN fill:#f39c12,color:#000
```

**Componentes principales:**

| Componente | Función |
|-----------|---------|
| **HDFS** | Sistema de archivos distribuido: divide archivos en bloques (128 MB) y los replica en múltiples nodos |
| **MapReduce** | Modelo de programación: procesa datos en paralelo (map → shuffle → reduce) |
| **YARN** | Gestiona recursos del clúster (CPU, memoria) y programa los jobs |

**Ventajas:**
- Probado a escala de petabytes
- Tolerante a fallos (replicación automática)
- Económico (hardware commodity)

**Desventajas:**
- Lento para procesamiento iterativo (escribe a disco entre cada paso)
- Curva de aprendizaje pronunciada
- Reemplazado por Spark en muchos casos

---

### Spark

Spark es el **sucesor moderno** de Hadoop MapReduce. Procesa datos en **memoria** (RAM), lo que lo hace **10-100× más rápido**.

```mermaid
flowchart LR
    subgraph "Procesamiento MapReduce"
        MR["Leer → Escribir → Leer → Escribir<br/>📀 → 💿 → 📀 → 💿<br/>Lento (disco)"]
    end
    
    subgraph "Procesamiento Spark"
        SP["Leer → Procesar → Procesar → Escribir<br/>📀 → 💾 → 💾 → 💿<br/>Rápido (memoria)"]
    end
    
    style MR fill:#e74c3c,color:#fff
    style SP fill:#2ecc71,color:#fff
```

**Componentes de Spark:**

```mermaid
flowchart TD
    SPARK["Apache Spark"]
    
    SPARK --> CORE["Spark Core<br/>Motor base, RDDs"]
    SPARK --> SQL["Spark SQL<br/>Consultas SQL<br/>estructuradas"]
    SPARK --> STREAMING["Spark Streaming<br/>Tiempo real"]
    SPARK --> ML["MLlib<br/>Machine Learning"]
    SPARK --> GRAPH["GraphX<br/>Procesamiento de<br/>grafos"]
    
    style SPARK fill:#e74c3c,color:#fff
    style CORE fill:#3498db,color:#fff
    style SQL fill:#2ecc71,color:#fff
    style STREAMING fill:#f39c12,color:#000
    style ML fill:#9b59b6,color:#fff
    style GRAPH fill:#1abc9c,color:#fff
```

**Ejemplo de Spark en Python (PySpark):**

```python
from pyspark.sql import SparkSession

# Iniciar sesión Spark
spark = SparkSession.builder.appName("Analisis").getOrCreate()

# Leer datos (distribuido, no importa el tamaño)
df = spark.read.csv("ventas/*.csv", header=True, inferSchema=True)

# Transformaciones (perezosas hasta que se necesita)
df.createOrReplaceTempView("ventas")
resultado = spark.sql("""
    SELECT region, SUM(monto) as total
    FROM ventas
    WHERE fecha >= '2025-01-01'
    GROUP BY region
    ORDER BY total DESC
""")

# Acción (aquí se ejecuta)
resultado.show()
```

**¿Hadoop o Spark?**

| Aspecto | Hadoop MapReduce | Spark |
|---------|-----------------|-------|
| **Velocidad** | Lento (disco) | Rápido (memoria) |
| **Facilidad** | ⭐⭐ | ⭐⭐⭐⭐ |
| **ML incluido** | No | Sí (MLlib) |
| **Streaming** | No nativo | Sí (Spark Streaming) |
| **Madurez** | Muy maduro | Maduro |
| **Cuándo usarlo** | Datos masivos + bajo presupuesto | Velocidad + ML + análisis interactivo |

---

## Técnicas de procesamiento de datos

### Procesamiento Paralelo

La idea fundamental de Big Data: **divide y vencerás**. Un problema grande se divide en partes pequeñas que se procesan simultáneamente.

```mermaid
flowchart TD
    INPUT["📥 10 TB de datos"]
    
    INPUT --> SPLIT["✂️ Dividir en partes<br/>100 partes de 100 GB"]
    
    SPLIT --> P1["💻 Parte 1"]
    SPLIT --> P2["💻 Parte 2"]
    SPLIT --> P3["💻 Parte 3"]
    SPLIT --> PN["💻 ... Parte N"]
    
    P1 --> R1["Resultado 1"]
    P2 --> R2["Resultado 2"]
    P3 --> R3["Resultado 3"]
    PN --> RN["Resultado N"]
    
    R1 --> MERGE["🔄 Combinar<br/>resultados"]
    R2 --> MERGE
    R3 --> MERGE
    RN --> MERGE
    
    MERGE --> OUTPUT["✅ Resultado final"]
    
    style INPUT fill:#3498db,color:#fff
    style SPLIT fill:#e67e22,color:#fff
    style MERGE fill:#9b59b6,color:#fff
    style OUTPUT fill:#2ecc71,color:#fff
```

### MPI (Message Passing Interface)

MPI es un estándar para **comunicación entre procesos** en sistemas de cómputo paralelo. Permite que múltiples computadoras (o núcleos) intercambien mensajes mientras trabajan en un problema.

**Conceptos clave:**

| Concepto | Definición |
|----------|-----------|
| **Proceso** | Una unidad de ejecución independiente |
| **Comunicador** | Grupo de procesos que pueden comunicarse |
| **Punto a punto** | Un proceso envía, otro recibe |
| **Colectiva** | Todos los procesos participan (broadcast, reduce) |

```python
# Pseudocódigo conceptual de MPI
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()   # ID de este proceso
size = comm.Get_size()   # Total de procesos

# Cada proceso trabaja en su parte
datos = cargar_parte_datos(rank, size)
resultado_parcial = procesar(datos)

# Combinar resultados (reducción)
resultado_total = comm.reduce(resultado_parcial, op=MPI.SUM, root=0)

if rank == 0:
    print(f"Resultado final: {resultado_total}")
```

**¿Cuándo usar MPI?** Problemas que requieren mucha comunicación entre procesos (simulaciones científicas, modelado climático, física computacional).

---

### MapReduce

MapReduce es el **modelo de programación** que popularizó Google (y luego Hadoop). Tiene dos fases:

```mermaid
flowchart LR
    INPUT["📥 Datos crudos"] --> MAP["🔀 MAP<br/>Procesar y emitir<br/>pares (clave, valor)"]
    MAP --> SHUFFLE["🔗 SHUFFLE<br/>Ordenar y agrupar<br/>por clave"]
    SHUFFLE --> REDUCE["📊 REDUCE<br/>Combinar valores<br/>por clave"]
    REDUCE --> OUTPUT["✅ Resultado"]
    
    style INPUT fill:#3498db,color:#fff
    style MAP fill:#e67e22,color:#fff
    style SHUFFLE fill:#f39c12,color:#000
    style REDUCE fill:#9b59b6,color:#fff
    style OUTPUT fill:#2ecc71,color:#fff
```

#### Ejemplo clásico: Word Count

```python
# FASE MAP: Divide el texto en palabras (cada palabra → 1)
def map(linea):
    palabras = linea.lower().split()
    for palabra in palabras:
        emit(palabra, 1)  # ("hola", 1), ("mundo", 1), ...

# SHUFFLE (automático): Agrupa por palabra
# ("hola", [1, 1, 1]), ("mundo", [1]), ...

# FASE REDUCE: Suma los contadores
def reduce(palabra, valores):
    total = sum(valores)
    emit(palabra, total)  # ("hola", 3), ("mundo", 1)
```

```python
# En la práctica con Spark esto es:
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("WordCount").getOrCreate()

rdd = spark.sparkContext.textFile("libro.txt")

word_counts = (rdd
    .flatMap(lambda linea: linea.lower().split())  # MAP
    .map(lambda palabra: (palabra, 1))             # MAP
    .reduceByKey(lambda a, b: a + b)               # REDUCE
)

word_counts.show()
```

### Procesamiento Batch vs Streaming

```mermaid
flowchart TD
    SUBQ["¿Cómo llegan los datos?"]
    
    SUBQ --> BATCH["📦 Por lotes<br/>(cada hora, cada día)"]
    SUBQ --> STREAM["🌊 Continuamente<br/>(tiempo real)"]
    
    BATCH --> B1["Técnica: MapReduce, Spark SQL<br/>Herramientas: Hadoop, Hive, Spark"]
    BATCH --> B2["Ideal para: Reportes diarios,<br/>procesamiento histórico"]
    
    STREAM --> S1["Técnica: Spark Streaming, Kafka<br/>Herramientas: Kafka, Flink, Storm"]
    STREAM --> S2["Ideal para: Detección de fraude,<br/>monitoreo en vivo, dashboards"]
    
    style SUBQ fill:#3498db,color:#fff
    style BATCH fill:#3498db,color:#fff
    style STREAM fill:#e74c3c,color:#fff
```

---

## Flujo completo de una arquitectura Big Data

```mermaid
flowchart LR
    SOURCES["📡 Fuentes<br/>APIs, IoT,<br/>Logs, DBs"] --> INGEST["📥 Ingesta<br/>Kafka, Flume,<br/>Kinesis"]
    INGEST --> STORE["🗄️ Almacenar<br/>HDFS / S3<br/>Data Lake"]
    STORE --> PROCESS["⚙️ Procesar<br/>Spark / Hadoop<br/>Hive / Presto"]
    PROCESS --> ANALYZE["📐 Analizar<br/>ML / SQL /<br/>Streaming"]
    ANALYZE --> VIS["📊 Visualizar<br/>Tableau / Power BI<br/>Superset"]
    
    style SOURCES fill:#3498db,color:#fff
    style INGEST fill:#e67e22,color:#fff
    style STORE fill:#f39c12,color:#000
    style PROCESS fill:#e74c3c,color:#fff
    style ANALYZE fill:#9b59b6,color:#fff
    style VIS fill:#2ecc71,color:#fff
```

---

## ¿Cuándo necesitas Big Data?

```mermaid
flowchart TD
    Q["❓ ¿Tus datos caben<br/>en una sola máquina?"]
    
    Q -->|Sí| NORMAL["✅ Usa herramientas normales<br/>SQL, pandas, R, Excel"]
    Q -->|No| BIG["❓ Big Data<br/>¿Qué necesitas?"]
    
    BIG --> ALM["Almacenar grandes<br/>volúmenes"]
    ALM --> ALM1["HDFS, S3,<br/>Data Lake"]
    
    BIG --> PROC["Procesar en<br/>lotes (batch)"]
    PROC --> PROC1["Hadoop, Spark,<br/>Hive"]
    
    BIG --> STREAM["Procesar en<br/>tiempo real"]
    STREAM --> STREAM1["Kafka, Spark Streaming,<br/>Flink"]
    
    BIG --> SQL["Hacer SQL sobre<br/>petabytes"]
    SQL --> SQL1["Presto, Hive,<br/>BigQuery, Snowflake"]
    
    style Q fill:#3498db,color:#fff
    style NORMAL fill:#2ecc71,color:#fff
    style BIG fill:#e74c3c,color:#fff
```

> 📁 **Ver también**: [[machine-learning]], [[recoleccion-de-datos]], [[introduccion-analisis-de-datos]]

## Relacionados:
- [[machine-learning]] #anterior 
- [[deep-learning]] #siguiente 