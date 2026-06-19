---
tipo: desafio
dificultad: 5
---

# 📊 Challenge Data Analytics — Integración de Fuentes de Datos

---

## 🎯 Objetivo

Crear un proyecto que consuma datos de **3 fuentes distintas** para poblar una base de datos con información cultural sobre bibliotecas, museos y salas de cines.

---

## 📚 Conocimientos Previos

- Conceptos de extracción, transformación y carga (ETL) de datos
- Trabajo con APIs y descarga de archivos mediante HTTP
- Procesamiento y limpieza de datos estructurados (CSV, JSON)
- Modelado de datos y diseño de esquemas de base de datos
- Conceptos de normalización y desnormalización de datos
- Técnicas de manejo de valores faltantes y duplicados
- Principios de reproducibilidad y versionado de datos

---

## 🧩 Funcionalidades

### Extracción de Datos
- Descargar archivos desde fuentes externas usando protocolos HTTP
- Almacenar archivos localmente siguiendo una estructura organizativa definida
- Manejar reemplazos de archivos existentes basado en fechas de descarga

### Transformación de Datos
- Normalizar datos heterogéneos de diferentes fuentes en un esquema unificado
- Procesar y limpiar datos: manejo de valores faltantes, estandarización de formatos
- Calcular métricas agregadas: registros por categoría, provincia y combinaciones
- Generar tablas de resumen para análisis específico (ej: cines con pantallas y butacas)

### Carga de Datos
- Crear esquemas de base de datos apropiados para los datos procesados
- Actualizar bases de datos reemplazando registros existentes
- Mantener trazabilidad mediante metadatos (fecha de carga, origen)
- Manejo apropiado de tipos de datos y restricciones de integridad

### Monitoreo y Logging
- Implementar registro detallado de cada paso del proceso
- Manejo de errores y excepciones con reporting apropiado
- Generación de reportes de ejecución y métricas de desempeño

---

## 💡 Sugerencias de Investigación

Para resolver este desafío, se recomienda investigar sobre:

- Patrones de diseño para procesos ETL: Pipeline Pattern, Batch Processing
- Estrategias de integración de datos: Data Virtualization, Data Federation
- Técnicas de normalización avanzada y modelado dimensional
- Algoritmos de deduplicación y record linkage
- Marcos de trabajo para procesamiento de datos: Apache Spark, Dask
- Principios de gobernanza y calidad de datos: DAMA-DMBOK
- Técnicas de versionado de datos: Data Lakehouse, Delta Lake
- Estrategias de manejo de esquemas evolutivos: Schema Evolution, Avro/Parquet
- Buenas prácticas en documentación de datos: Data Catalogs, Business Glossary
- Técnicas de validación y testing de pipelines de datos: Great Expectations

---

## 📖 Documentación

Documentar el proceso completo incluyendo:
- Arquitectura de la solución y flujo de datos
- Esquemas de base de datos y relaciones
- Instrucciones de instalación y configuración
- Ejemplos de uso y casos de prueba
- Mantenimiento y operación del sistema

---

## 🧪 Tests (Opcional)

Agregar pruebas para verificar:
- Extracción correcta de datos desde fuentes externas
- Transformaciones de datos: normalización, limpieza, agregación
- Carga adecuada en base de datos: integridad, reemplazos, metadatos
- Manejo de casos edge: archivos vacíos, formatos incorrectos, conexiones fallidas
- Rendimiento y escalabilidad del proceso ETL

---

## ✅ Criterios de evaluación

| Criterio | Descripción |
|----------|-------------|
| 🔧 Arquitectura | Diseño limpio y modular del proceso ETL |
| 📊 Calidad de Datos | Precisión, completitud y consistencia de los datos procesados |
| 🔄 Reproducibilidad | Capacidad de ejecutar el proceso múltiples veces con resultados consistentes |
| 📋 Documentación | Documentación completa y actualizada del sistema |
| 🧪 Testing | Cobertura adecuada de pruebas automatizadas |
| ⚙️ Eficiencia | Optimización en uso de recursos y tiempo de procesamiento |
| 📈 Escalabilidad | Capacidad para manejar volúmenes de datos crecientes |

---

## 🔗 Challenges y retos similares
- [[plataforma-challenges/challenge-backend-node]] — Backend Node
- [[plataforma-challenges/challenge-frontend-angular]] — Frontend Angular
- [[plataforma-challenges/challenge-fullstack-js]] — Fullstack JS
- [[retos/04-reportes]] — Reportes CSV
- [[retos/06-2-graficador-financiero]] — Gráficas Matplotlib
- [[retos/reto-graficas-excel]] — Visualización Java
- [[retos/reto-mer]] — Modelado de datos
- [[../midudev-javascript/05-optimizando-viajes]] — Optimización