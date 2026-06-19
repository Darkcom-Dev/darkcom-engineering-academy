# Curso intensivo de 5 días para agentes de IA con Google
Kaggle-Mentor3

Nuestro nuevo Agentes de IA de 5 días: curso intensivo de codificación de vibraciones con Google ¡está aquí!
¿Listo para crear agentes? Únase a la Curso 2026 ahora!

## ¿Cuál es el curso intensivo de 5 días para agentes de IA?

El Curso intensivo de 5 días para agentes de IA con Google es un programa práctico que originalmente se llevó a cabo en vivo del 10 al 14 de noviembre de 2025. Ahora está disponible como una guía de Kaggle Learn a su propio ritmo para que cualquiera pueda explorar los fundamentos, la arquitectura y el desarrollo práctico de los agentes de IA.

Este curso fue elaborado por investigadores e ingenieros de ML de Google para ayudar a los desarrolladores a explorar los fundamentos y las aplicaciones prácticas de los agentes de IA. Aprenderás los componentes principales – modelos, herramientas, orquestación, memoria y evaluación. Finalmente, descubrirás cómo los agentes van más allá de los prototipos LLM para convertirse en sistemas listos para producción.

Cada día combina inmersiones conceptuales profundas con ejemplos prácticos, laboratorios de códigos y debates en vivo. Al final, estarás listo para crear, evaluar e implementar agentes que resuelvan problemas del mundo real.

¡Bienvenido a nuestra Guía de aprendizaje intensivo para agentes!
Otros recursos

## ¿Qué se está cubriendo?

- Día 1 – Introducción a los agentes: Explore los conceptos fundamentales de los agentes de IA, sus características definitorias y cómo las arquitecturas agentónicas se diferencian de las aplicaciones LLM tradicionales, sentando las bases para construir sistemas inteligentes y autónomos.

- Día 2 - Herramientas del agente e interoperabilidad con el protocolo de contexto del modelo (MCP): Sumérjase en el mundo de las herramientas, comprendiendo cómo los agentes de IA pueden "actuar" aprovechando funcionalidades externas y API, y explore la facilidad de descubrir y utilizar las herramientas que ofrece el MCP.

- Día 3 - Ingeniería de contexto: sesiones y memoria: Explore cómo crear agentes de IA que puedan recordar interacciones pasadas y mantener el contexto. Aprenda a implementar memoria a corto y largo plazo para crear agentes más robustos capaces de manejar tareas complejas de múltiples turnos.

- Día 4 – Calidad del agente: Aprenda a crear agentes de IA sólidos y confiables dominando las disciplinas críticas de evaluar y mejorar agentes. Esta sesión cubrirá la observabilidad, el registro y el rastreo para brindar visibilidad, junto con métricas clave y estrategias de evaluación para optimizar el rendimiento del agente.

- Día 5 – Prototipo a producción: Vaya más allá de las pruebas locales y aprenda a implementar y escalar agentes de IA para su uso en el mundo real. Esta sesión cubrirá las mejores prácticas para implementar sus agentes para que otros puedan usarlos, incluido cómo crear un sistema verdaderamente multiagente con el Protocolo Agent2Agent (A2A).
Otros recursos

## Instrucciones de configuración
Para asegurarse de estar listo para el curso, complete estos pasos de configuración esenciales:

 Kaggle cuenta: Regístrese para obtener una cuenta de Kaggle y Aprenda cómo funcionan los cuadernos. Asegúrese de verificar teléfono tu cuenta, es necesaria para los laboratorios de códigos del curso.
 Estudio de IA cuenta: Regístrese para obtener una cuenta de AI Studio y asegúrese de poder generar una Clave API.
 Discordia de Kaggle: Regístrese para obtener una cuenta de Discord y únase a nosotros en el servidor de Discord de Kaggle para conectarse y colaborar con otros estudiantes en este curso.
Otros recursos

### Día 1 (Introducción a los agentes)
Bienvenidos al día 1.

Este documento técnico presenta los agentes Al. Presenta una taxonomía de las capacidades de los agentes, enfatiza la necesidad de una disciplina de Agent Ops para la confiabilidad y la gobernanza, y analiza la importancia de la interoperabilidad y la seguridad de los agentes a través de políticas de identidad y restringidas.

En los laboratorios de códigos, construirá su primer agente de IA y su primer sistema multiagente, utilizando Agent Development Kit (ADK), desarrollado por Gemini, y le dará la capacidad de usar la Búsqueda de Google para responder preguntas con información actualizada. En el segundo codelab, la atención se centrará en sistemas multiagente, donde aprenderá a crear equipos de agentes especializados y explorar diferentes patrones arquitectónicos.

#### Tareas del día 1

1. Complete la Unidad 1 – “Introducción a los Agentes”:

- [ ]  Escuche el resumen del episodio del podcast para esta unidad.
- [ ]  Para complementar el podcast, lea el “Introducción a los agentes ” documento técnico
Completa estos laboratorios de códigos en Kaggle:
- [ ]  Construir tu primer agente usando Gemini y ADK.
- [ ]  Construir sus primeros sistemas multiagente que utilizan ADK.
- [ ]  Asegúrate de verificar teléfono tu cuenta de Kaggle antes de comenzar, es necesaria para los laboratorios de códigos.
- [ ]  También tenemos un guía de solución de problemas para los laboratorios de códigos. Asegúrese de consultar allí para encontrar soluciones a problemas comunes.
 Quiero tener un conversación interactiva? Intente agregar los documentos técnicos a CuadernoLM.

[Opcional]: Mira lo grabado Transmisión en vivo de YouTube. Kanchana Patlolla y Anant Nawalgaria organizó la primera transmisión en vivo en el canal de YouTube de Kaggle. A ellos se unieron los autores del codelab Kristopher Overholt y Hangfei Lin, junto con invitados especiales de Google: Alan Blount, Mike Clark, Michael Gerstenhaber y Antonio Gulli para discutir las tareas y compartir ideas.
Otros recursos

### Día 2 (Herramientas de agente e interoperabilidad con el protocolo de contexto de modelo (MCP))
Bienvenidos al día 2.

Este documento técnico se centra en las funciones de herramientas externas que permiten a un agente realizar acciones o recuperar datos en tiempo real más allá de su conjunto de entrenamiento e introduce las mejores prácticas para diseñar herramientas efectivas. Aprenderá sobre MCP, destacando sus componentes arquitectónicos, capa de comunicación, riesgos y brechas de preparación empresarial.

En los laboratorios de código, creará herramientas personalizadas para sus agentes convirtiendo sus propias funciones de Python en acciones que su agente puede realizar. También utilizará MCP e implementará operaciones de larga duración donde un agente puede pausar las llamadas a herramientas mientras espera la aprobación humana, antes de reanudarlas.

#### Tareas del día 2:

Unidad completa 2 - “Herramientas del agente e interoperabilidad con el protocolo de contexto del modelo (MCP)”:

- [ ]  Escuche el resumen del episodio del podcast para esta unidad.
- [ ]  Para complementar el podcast, lea el “Libro blanco sobre herramientas de agentes e interoperabilidad con el protocolo de contexto de modelo (MCP)”.
Completa estos laboratorios de códigos en Kaggle:
- [ ]  Explorar Nuevas formas de agregar herramientas para ampliar lo que sus agentes pueden hacer.
- [ ]  Explorar mejores prácticas para herramientas, incluido el uso de MCP y operaciones de larga duración.

[Opcional]: Mira lo grabado Transmisión en vivo de YouTube. A Kanchana Patlolla y Anant Nawalgaria se unieron la autora del codelab Laxmi Harikumar, junto con los invitados de Google Edward Grefenstette, Mike Styer y Oriol Vinyals, así como el orador externo Alex Wissner-Gross de Reificado, para discutir las tareas y compartir ideas.
Otros recursos

### Día 3 (Ingeniería contextual: sesiones y memoria)
Bienvenidos al día 3.

Este documento técnico explora la ingeniería de contexto como la práctica de ensamblar y administrar dinámicamente información dentro de la ventana de contexto de un agente para crear experiencias Al personalizadas y con estado. Define las Sesiones como el contenedor del historial de una conversación única e inmediata, y la Memoria como el mecanismo de persistencia a largo plazo.

En los laboratorios de código, aprenderá cómo hacer que los agentes tengan estado administrando el historial de conversaciones a través de ingeniería de contexto en ADK y la memoria de trabajo dentro de una sesión, lo que permite a su agente recordar el contexto y tener conversaciones coherentes de varios turnos. En el segundo cuaderno, le entregarás a tu agente una memoria a largo plazo que persiste en diferentes sesiones.

#### Tarea del día 3

Unidad Completa 3 - “Ingeniería de Contexto: Sesiones y Memoria”:

- [ ] Escuche el resumen del episodio del podcast para esta unidad.
- [ ]  Para complementar el podcast, lea el “Ingeniería de contexto: documento técnico sobre sesiones y memoria”.
Completa estos laboratorios de códigos en Kaggle:
- [ ]  Construir agentes con estado y realizan ingeniería de contexto.
- [ ]  Explorar cómo utilizar la memoria con su agente.

[Opcional]: Mira lo grabado Transmisión en vivo de YouTube. Kanchana Patlolla y Anant Nawalgaria estuvieron acompañados por el autor del codelab Kristopher Overholt, junto con invitados de Google: Steven Johnson, Kimberly Milam y Julia Wiesinger, y el orador externo Jay Alammar de Cohere.
Otros recursos

### Día 4 (Calidad del Agente)
Bienvenidos al día 4.

Este documento técnico aborda el desafío de garantizar la calidad de los agentes de Al mediante la introducción de un marco de evaluación holístico. La base técnica necesaria para esto es la Observabilidad, construida sobre tres pilares: Registros (el diario), Rastros (la narrativa) y Métricas (el informe de salud), lo que permite un ciclo de retroalimentación continuo utilizando métodos escalables como la evaluación LLM-as-a-Judge y Human-in-the-Loop (HITL).

En los laboratorios de código, aprenderá a utilizar registros, seguimientos y métricas para obtener visibilidad completa del proceso de toma de decisiones de su agente, lo que le permitirá depurar fallas y comprender por qué su agente se comporta como lo hace. En el segundo codelab, aprenderá cómo evaluar a sus agentes para calificar la calidad de respuesta de sus agentes y el uso de herramientas.

#### Tarea del día 4

Unidad completa 4 - “Calidad del agente”:

- [ ]  Escuche el resumen del episodio del podcast para esta unidad.
- [ ]  Para complementar el podcast, lea el Libro blanco sobre la calidad del agente.
Completa estos laboratorios de códigos en Kaggle:
- [ ]  Implementar observabilidad para ayudarle a depurar sus agentes.
- [ ]  Evaluar tus agentes.

[Opcional]: Mira lo grabado Transmisión en vivo de YouTube. A Kanchana Patlolla y Anant Nawalgaria se unieron la autora del codelab Sita Lakshmi Sangameswaran, junto con invitados de Google: Wafae Bakkali, Turan Bulmus y Sian Gooding, y el invitado externo Jiwei Liu de NVIDIA.
Otros recursos

### Día 5 (Prototipo de producción)
Bienvenidos al día 5.

Este documento técnico proporciona una guía técnica sobre el ciclo de vida operativo de los agentes de IA, centrándose en la implementación, el escalamiento y la producción. Explora los desafíos de la transición de sistemas agentes desde prototipos a soluciones de nivel empresarial, con especial atención al Protocolo Agent2Agent (A2A).

En los laboratorios de códigos, aprenderá cómo construir sistemas de múltiples agentes independientes que puedan comunicarse y colaborar utilizando el Protocolo A2A. También aprenderá cómo llevar su agente desde su máquina local a un servicio escalable y listo para producción, implementando su agente en Vertex AI Agent Engine en Google Cloud.

#### Tarea final

Unidad completa 5 - “Prototipo a producción”:

- [ ]  Escuche el resumen del episodio del podcast para esta unidad.
- [ ]  Para complementar el podcast, lea el “De prototipo a producción” documento técnico. .
Completa estos laboratorios de códigos en Kaggle:
- [ ]  Explorar cómo utilizar el protocolo A2A para que los agentes interactúen entre sí.
 [Opcional] Implementar su agente en Agent Engine en Google Cloud.

[Opcional]: Mira lo grabado Transmisión en vivo de YouTube. Kanchana Patlolla y Anant Nawalgaria estuvieron acompañados por la autora del codelab Laxmi Harikumar, junto con invitados de Google: Will Grannis, Sokratis Kartakis, Elia Secchi y Saurabh Tiwary.

---

¡Felicitaciones por completar la Guía de aprendizaje intensivo para agentes!
¡feliz Kaggling!