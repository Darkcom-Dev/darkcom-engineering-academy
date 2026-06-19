---
id: curso-de-desarrollo-con-ia
type: note
status: seed
domain: coding
created: 2025-05-15
updated: 2025-05-15
tags: [prompting, ai-workflows, desarrollo, mvp, seguridad]
up: "[[indice-desarrollo-ia]]"
---

# Aprendizaje con IA y Flujos de Trabajo Modernos

TL;DR: Recopilación de aprendizajes estratégicos sobre flujos de trabajo potenciados por IA, desde la gestión de fuentes con NotebookLM hasta la orquestación segura de agentes.

## 📚 Estrategias de Aprendizaje y Gestión de Fuentes con NotebookLM
La gestión de información es crítica para evitar alucinaciones y mantener la fidelidad técnica.
- **NotebookLM:** Herramienta central para centralizar conocimiento. Permite crear mapas mentales, tarjetas didácticas, resúmenes de video e informes técnicos basados en fuentes específicas.
- **Identificación del Stack:** La IA ayuda a comparar y seleccionar el stack tecnológico adecuado para cada proyecto basándose en requisitos de negocio.
- **Casos de Uso Operativo:** 
  - Depurador contextual.
  - Consultoría de APIs y frameworks.
  - Estrategias de migración y refactorización.
  - Análisis post-mortem de incidentes.

---

## 🏗️ Metodologías de Investigación y Refinamiento de Prompts
La ingeniería de prompts evoluciona hacia un proceso de investigación profunda.
- **Ciclo de Investigación:** Usar Deep Research para generar un informe base (PDF), procesarlo en NotebookLM y extraer estructuras de Markdown.
- **Visualización:** Pedir a los modelos que generen estructuras compatibles con herramientas de mapas mentales.
- **Recursos Externos:** Consultar guías oficiales como la de [[guia-de-prompts-de-anthropic]] #requires.

---

## 🚀 Despliegue y Prototipado Rápido con Firebase Studio
Firebase Studio permite pasar de la idea a la producción eliminando fricciones técnicas.
- **Workflow:** Crear un prompt extremadamente detallado que describa la tecnología y los elementos de la interfaz, y delegar la construcción a la suite de Firebase.

---

## 🧪 Estrategias de Desarrollo MVP y Generación de Tests (TDD)
La IA es el acelerador perfecto para productos mínimos viables.
- **TDD con IA:** Solicitar a la IA que genere primero los tests unitarios y luego la implementación necesaria para superarlos.
- **Arquitectura:** Es imperativo que el desarrollador guíe a la IA con conceptos de arquitectura (Clean Architecture, SOLID) para evitar deuda técnica prematura.

---

## 🔧 Optimización de Contexto y Gestión de Agentes Locales
Enseñar a la IA a entender el proyecto específico reduce drásticamente las alucinaciones.
- **Inyección de Contexto:** Proporcionar esquemas de base de datos y documentación técnica a través de carpetas dedicadas (`/context`, `/docs`).
- **Standard Agents.md:** Uso de un archivo central (`agents.md` o `GEMINI.md`) que defina:
  - Misión y alcance del proyecto.
  - Reglas de estilo y estándares de codificación.
  - Fuentes de verdad (documentación oficial, specs).

---

## 🎭 Orquestación Multimodelo y Delegación de Tareas
El desarrollo moderno implica coordinar múltiples agentes especializados.
- **Sub-agentes:** Creación de expertos dedicados (ej. Agente PM, Agente Arquitecto).
- **Claude Code:** Uso de herramientas de terminal avanzadas que permiten inicializar contextos locales (`.claude/claude.md`).
- **Multimodalidad:** Aprovechar la capacidad de la terminal para procesar imágenes (ej. capturas de pantalla de errores o bocetos de UI).

---

## 🛡️ Protocolos de Seguridad y Mitigación de Vulnerabilidades
La seguridad debe ser una prioridad desde el inicio del ciclo de vida (Security by Design).
- **Riesgos de Datos Públicos:** Los modelos pueden haber sido entrenados con datos vulnerables o sugerir prácticas inseguras.
- **Pruebas de Penetración:** Uso de herramientas como **Hydra** (ataques de diccionario) o **Dirb** (descubrimiento de archivos) para auditar la robustez del sistema generado.

---

## 🔗 Recursos y Sesiones Complementarias
- **Apuntes Diarios:** 
	- [[sesion-01-aprendizaje-notebooklm-prompts]] #related
	- [[sesion-02-arquitecto-orquestacion-agentes]] #related
	- [[sesion-03-automatizacion-n8n-workflows]] #related
- **Ingeniería de Agentes:** [[harness-engineering-guia]] #related.

## 🏁 Checklist de Calidad RAG
- [x] Frontmatter completo con metadatos de navegación.
- [x] TL;DR presente y conciso.
- [x] Títulos descriptivos para cada sección.
- [x] Enlaces semánticos con etiquetas de relación.
- [x] Estructura jerárquica clara y secciones autónomas.
