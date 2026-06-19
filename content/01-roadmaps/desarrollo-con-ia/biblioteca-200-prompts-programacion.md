---
id: 200-prompts-programacion
type: ebook
status: evergreen
domain: coding
created: 2025-05-15
updated: 2025-05-15
tags: [prompts, ia, desarrollo, cheat-sheet]
up: "[[indice-desarrollo-ia]]"
---

# 200 Prompts que todo Programador debe conocer

TL;DR: Compendio exhaustivo de 200 plantillas y estructuras de prompts optimizadas para cada fase del ciclo de vida del desarrollo de software, desde el aprendizaje hasta la arquitectura.

## 🔗 Navegación y Contexto
- **Uso práctico en:** [[sesion-01-aprendizaje-notebooklm-prompts]] #related, [[sesion-02-arquitecto-orquestacion-agentes]] #related
- **Índice Maestro:** [[indice-desarrollo-ia]] #parent

## 🧠 Estructura Fundamental del Prompt de Alta Calidad
Para obtener resultados de alta calidad, un prompt debe incluir estos elementos:

1. **Rol o Persona:** Define quién es la IA (ej. "Actúa como un desarrollador senior en backend").
2. **Objetivo o Tarea:** Qué necesitas exactamente (ej. "Crea una función que valide emails").
3. **Contexto:** Detalles del entorno (tecnologías, frameworks, propósito del proyecto).
4. **Instrucciones:** Pasos lógicos a seguir.
5. **Formato de Respuesta:** Cómo quieres ver el resultado (JSON, Markdown, bloque de código).
6. **Restricciones:** Límites (ej. "no uses librerías externas").
7. **Cláusula de Clarificación:** Invitar a la IA a preguntar si le falta información.

---

## ⚡ Catálogo de 100 Plantillas para Operaciones Diarias
*Plantillas diseñadas para tareas comunes a lo largo del ciclo de vida del desarrollo.*

### 1. Estudio y Fundamentos (1-20)
1. Explica con tus propias palabras qué es y cómo funciona la memoria dinámica en un programa (¿qué son el heap y el stack?).
2. ¿Cuál es la diferencia entre un arreglo (array) y una lista enlazada, y en qué casos usarías cada uno?
3. Define qué es la recursividad y proporciona un ejemplo sencillo de cómo se utiliza.
4. ¿Cómo funciona el algoritmo de búsqueda binaria y cuál es su complejidad temporal Big-O?
5. Explica el principio SOLID de Responsabilidad Única y por qué es importante en el diseño de software.
6. ¿Qué son las condiciones de carrera (race conditions) y cómo puedes prevenirlas en un programa concurrente?
7. Explica la diferencia entre procesos e hilos (threads) en un sistema operativo.
8. ¿Qué es un sistema de control de versiones (ej. Git) y por qué es esencial en el desarrollo de software?
9. Enumera las etapas principales del ciclo de vida de desarrollo de software (SDLC) y describe brevemente qué se hace en cada una.
10. ¿Cuál es la diferencia entre una prueba unitaria y una prueba de integración, y cuál es el propósito de cada una?
11. Actúa como un profesor de programación para principiantes. Explica [concepto, ej. variables en Python] paso a paso, con analogías del mundo real y un ejemplo simple de código. Usa viñetas para los pasos.
12. Soy un programador junior. Dame un tutorial corto sobre [tema, ej. bucles for en JavaScript], incluyendo sintaxis, errores comunes y 3 ejercicios prácticos con soluciones. Formato: Tabla para ejercicios.
13. Explica la diferencia entre [concepto_1] y [concepto_2, ej. listas vs tuplas en Python] como si se lo dijeras a un niño de 10 años. Luego, da ejemplos de código para cada uno.
14. Crea un mapa mental en texto para [tema, ej. programación orientada a objetos]. Incluye definiciones clave, relaciones y ejemplos en [lenguaje].
15. Lista 5 recursos gratuitos (libros, videos, sitios web) para aprender [tecnología, ej. React]. Para cada uno, describe por qué es bueno para juniors y qué cubre. Usa una tabla.
16. Explica cómo funciona [algoritmo, ej. quicksort] paso a paso con un ejemplo numérico. Incluye código en [lenguaje] y complejidad temporal.
17. Dame un glosario de 10 términos clave en [área, ej. bases de datos SQL] con definiciones simples y un ejemplo de uso. Formato: Tabla.
18. Simula una sesión de estudio: Pregúntame 5 preguntas de opción múltiple sobre [tema, ej. funciones en C++], y explica las respuestas correctas después.
19. Resume un artículo o documentación sobre [tema, ej. async/await en JavaScript]. Proporciona puntos clave, pros/contras y un ejemplo.
20. Crea un plan de estudio de una semana para aprender [lenguaje/framework, ej. FastAPI] desde cero. Incluye metas diarias y recursos.

### 2. Desarrollo e Implementación (21-40)
21. Escribe un pseudocódigo para calcular el factorial de un número dado de forma iterativa.
22. Implementa una función que determine si una cadena de texto es un palíndromo (ignorando mayúsculas, espacios y acentos).
23. Diseña un algoritmo para ordenar una lista de números utilizando el método de ordenamiento burbuja (Bubble Sort).
24. Crea una función que reciba una lista de números y devuelva la suma de todos los números pares en la lista.
25. Describe cómo conectarías una base de datos a una aplicación sin usar frameworks, mencionando los pasos principales (conexión, consultas, cierre).
26. Escribe un fragmento de código que abra un archivo de texto y lea su contenido línea por línea (en [lenguaje]).
27. ¿Cómo manejarías las excepciones o errores en un programa para evitar que este se cierre inesperadamente? (Estrategia general).
28. Implementa una función que realice una búsqueda de un número en una lista (array) y retorne su índice si lo encuentra o -1 si no está presente.
29. Crea una estructura de datos pila (stack) básica usando un array, con funciones para apilar (push) y desapilar (pop) elementos.
30. Traduce el siguiente fragmento de código de Python a JavaScript manteniendo la misma funcionalidad: [código].
31. Actúa como un desarrollador senior. Genera código en [lenguaje] para [tarea]. Incluye comentarios y maneja errores. Formato: Bloque de código.
32. Crea una clase en [lenguaje] para [objeto] con atributos, métodos y herencia. Explica cada parte paso a paso.
33. Escribe un script completo en [lenguaje] para [proyecto pequeño, ej. un juego de adivinanza]. Usa mejores prácticas e incluye pruebas.
34. Genera una API REST simple en [framework, ej. Express.js] con endpoints para CRUD en una lista de tareas.
35. Crea un componente React para [elemento, ej. formulario de login]. Incluye estado, props y estilos básicos. Explica el flujo.
36. Implementa un patrón de diseño [patrón, ej. singleton] en [lenguaje]. Da un ejemplo real y por qué usarlo.
37. Escribe código para leer/escribir archivos en [lenguaje] manejando excepciones. Incluye un ejemplo con datos JSON.
38. Genera una consulta SQL para [tarea, ej. unir dos tablas]. Explica la sintaxis y posibles optimizaciones.
39. Crea un loop que procese [datos] para calcular estadísticas básicas. Usa bibliotecas si aplica.
40. Desarrolla un frontend simple con HTML/CSS/JS para [página, ej. un contador]. Hazlo responsive y accesible.

### 3. Depuración e Identificación de Errores (41-55)
41. Tienes un programa que se queda en un bucle infinito. ¿Qué pasos seguirías para encontrar la causa y resolverlo?
42. El código arroja un error NullPointerException al ejecutarse. ¿Cómo identificarías dónde ocurre el null y cómo lo solucionarías?
43. ¿Cómo usarías un depurador (debugger) para inspeccionar el estado de un programa paso a paso mientras se ejecuta?
44. Tu aplicación web devuelve un error 500 (Internal Server Error) sin detalles. ¿Cómo procederías para encontrar la causa raíz?
45. Tienes una función que debería ejecutarse en segundos, pero tarda minutos. ¿Qué técnicas usarías para identificar el cuello de botella?
46. Al ejecutar un script, este no produce ninguna salida ni error. ¿Cómo investigarías qué está ocurriendo?
47. Describe un método para encontrar posibles fugas de memoria (memory leaks) en una aplicación que corre durante horas.
48. Un programa lanza una excepción de "índice fuera de rango". ¿Qué podría estar causando este error y cómo lo solucionarías?
49. Tu programa compila y corre, pero el resultado es incorrecto. ¿Cómo aislarías la parte del código con el error lógico?
50. Menciona tres técnicas o buenas prácticas generales para depurar errores de manera más eficiente.
51. Tengo este error: [descripción]. Aquí está mi código: [código]. Explica por qué ocurre y cómo solucionarlo paso a paso.
52. Depura este código en [lenguaje]: [código]. Identifica bugs potenciales y sugiere correcciones con explicaciones.
53. Explica técnicas de depuración en [lenguaje, ej. con debugger en VS Code]. Dame pasos para un ejemplo simple.
54. Mi programa se cuelga en [escenario]. Analiza este código: [código] y propone soluciones.
55. Compara errores comunes en [lenguaje1] vs [lenguaje2]. Lista 5 por cada uno con fixes. Formato: Tabla.

### 4. Estrategias de Pruebas y Control de Calidad (56-69)
56. Escribe casos de prueba unitarios para una función hipotética esPrimo(n) que verifica si un número es primo.
57. ¿Cómo probarías la funcionalidad de registro de usuarios en una aplicación web? (Casos de prueba principales).
58. Genera datos de prueba (dummy data) para validar una aplicación de gestión de estudiantes (incluye valores atípicos).
59. Explica en qué consiste el Desarrollo Guiado por Pruebas (TDD) y cómo se aplicaría en un proyecto pequeño.
60. La aplicación falla cuando se introduce un carácter especial (ej. ñ o @). ¿Cómo crearías una prueba para ese caso?
61. ¿Qué es una prueba de regresión y por qué es importante ejecutarla después de realizar cambios o corregir un bug?
62. Describe la diferencia entre pruebas automatizadas y pruebas manuales. ¿Cuándo conviene usar cada una?
63. ¿Qué métricas o herramientas usarías para medir la calidad del código (ej. cobertura, análisis estático)?
64. Diseña casos de prueba para una función calculadora que realiza divisiones, incluyendo división por cero.
65. Escribe tests unitarios en [framework, ej. Jest] para esta función: [función]. Cubre casos edge y normales.
66. Explica qué es testing [tipo, ej. integración] y cómo implementarlo en [lenguaje]. Da un ejemplo.
67. Revisa este código para calidad: [código]. Sugiere mejoras en legibilidad, eficiencia y estándares (ej. PEP8).
68. Genera mocks para testing en [lenguaje, ej. con unittest en Python]. Usa un ejemplo con APIs externas.
69. Lista 5 mejores prácticas para escribir código testable. Explica cada una con un ejemplo corto.

### 5. Optimización de Rendimiento y Documentación Técnica (70-79)
70. Optimiza este código en [lenguaje]: [código]. Reduce complejidad temporal y explica cambios.
71. Explica profiling en [lenguaje, ej. con cProfile en Python]. Dame pasos para analizar un script.
72. Sugiere formas de optimizar [tarea, ej. consultas SQL lentas]. Incluye ejemplos antes/después.
73. Compara algoritmos para [problema] en términos de rendimiento. Recomienda uno para [escenario]. Formato: Tabla.
74. Optimiza memoria en [lenguaje, ej. garbage collection en Java]. Da tips y un ejemplo.
75. Genera documentación para esta función: [función]. Usa formato docstring y explica parámetros/retorno.
76. Crea un README.md para un proyecto [descripción]. Incluye instalación, uso y contribuciones.
77. Revisa este pull request: [descripción]. Sugiere feedback constructivo en viñetas.
78. Explica cómo usar [herramienta, ej. JSDoc] para documentar código. Da un ejemplo.
79. Lista estándares de codificación para [lenguaje, ej. Google Style Guide]. Resume 10 reglas clave.

### 6. Diseño de Sistemas, Escalabilidad y Mejores Prácticas (80-100)
80. ¿Cómo incorporarías pruebas de seguridad (ej. inyección SQL, XSS) en el proceso de testing web?
81. Diseña la arquitectura de un sistema de chat en tiempo real para miles de usuarios (componentes clave).
82. ¿Qué patrón aplicarías para extender un módulo sin modificar su código (Open/Closed)? Ejemplo práctico.
83. Recomienda una pila tecnológica para un e-commerce básico explicando la elección de cada componente.
84. Menciona mejores prácticas para escribir código limpio y mantenible (variables, funciones, DRY).
85. ¿Qué información clave incluirías en un buen archivo README de un repositorio de código?
86. Al revisar código ajeno, ¿en qué aspectos te fijarías para asegurar la calidad y qué sugerirías mejorar?
87. ¿Cómo abordarías la escalabilidad de una web si anticipas un crecimiento del 1000% (DB, balanceo, caché)?
88. Describe la importancia de la modularidad y cómo la aplicarías para organizar un proyecto grande.
89. ¿Qué harías para garantizar que tu equipo sigue una guía de estilos consistente (linters, revisiones)?
90. ¿Qué consideraciones de diseño y seguridad tendrías al integrar una API de terceros?
91. Prepara preguntas de entrevista para [rol]. Incluye 5 técnicas y 5 de código con soluciones.
92. Configura un entorno de desarrollo para [stack, ej. MERN]. Lista pasos, herramientas y comandos.
93. Dame tips para debugging en producción vs desarrollo. Incluye herramientas como logs.
94. Explica Agile/Scrum para juniors. Describe roles, ceremonias y aplicación en proyectos personales.
95. Sugiere un workflow Git para principiantes (branching, commits, merges) con diagramas en texto.
96. Lista 10 hábitos diarios para mejorar como programador junior. Explica cada uno brevemente.
97. Recomienda extensiones VS Code para [lenguaje]. Describe qué hace cada una. Formato: Tabla.
98. Explica seguridad básica en código: [tema, ej. SQL injection]. Da prevención y ejemplos.
99. Crea un plan para refactorizar código legacy: [descripción]. Pasos secuenciales y riesgos.
100. Dame consejos para equilibrar aprendizaje y programación en una carrera junior (metas realistas).

---

## 🛠️ Colección de 50 Prompts para Tareas Estándar
*Prompts categorizados para tareas habituales desde un enfoque simple.*

### 1. Comprensión Conceptual y Aprendizaje Acelerado (1-10)
1. **Explicar un concepto técnico:** Actúa como un profesor de CS especializado en simplificar temas. Explica [concepto] a un junior con una analogía, pseudocódigo y lista de pros/contras.
2. **Comparar tecnologías:** Actúa como analista objetivo. Crea una tabla comparando [T1] y [T2] según [criterios] y casos de uso ideales.
3. **Traducir código:** Actúa como programador políglota. Traduce [código] de forma idiomática al lenguaje de destino con comentarios explicativos.
4. **Resumir documentación:** Actúa como editor técnico. Resume [texto] en 5 puntos clave enfocados en uso rápido.
5. **Explicar código complejo:** Actúa como senior en revisión. Explica [código] línea por línea, detallando lógica, interacción y patrones.
6. **Explicar error:** Actúa como depurador experto. Dado el error [mensaje], explica causa raíz y da 3 soluciones por probabilidad.
7. **Identificar propósito:** Analiza [script], describe su fin en una frase y detalla pasos secuenciales.
8. **Recursos de aprendizaje:** Actúa como curador educativo. Recomienda los 3 mejores recursos para aprender [tema] y por qué.
9. **Preguntas de entrevista:** Actúa como entrevistador. Genera 5 preguntas junior (conceptos + pseudocódigo) sobre [tema].
10. **Complejidad algorítmica:** Analiza [función], determina su Big O en el peor caso y explica razonamiento paso a paso.

### 2. Generación de Código e Implementación de Componentes (11-25)
11. **Función con requisitos:** Genera función en [lenguaje] que [tarea] con manejo de errores, complejidad óptima y docstrings.
12. **Clase o módulo:** Diseña clase [nombre] en [lenguaje] para gestionar [recurso] con constructor, métodos públicos/privados y encapsulación.
13. **Regex:** Actúa como experto. Crea regex para validar [patrón] compatible con [motor] con explicación detallada.
14. **Consulta base de datos:** Actúa como DBA. Escribe SQL optimizado para [objetivo] usando tablas [T1] y [T2].
15. **Archivo configuración:** Genera config para proyecto [tecnología] bien comentado.
16. **Mocks/Fixtures:** Genera array JSON de 5 objetos realistas para [dominio] con claves [K1, K2, K3].
17. **Algoritmo clásico:** Implementa [algoritmo] en [lenguaje] con documentación y ejemplo de uso.
18. **Código desde comentarios:** [lenguaje]. Función que: reciba lista -> filtre pares -> eleve al cuadrado -> sume total.
19. **Completar función:** Completa la lógica de: [fragmento de código incompleto].
20. **Endpoint REST:** Genera boilerplate en [tecnología] para GET en "/api/items" devolviendo JSON y status 200.
21. **Estructura de datos:** Define [struct/class] en [lenguaje] para [entidad] con atributos [lista].
22. **Utilidad común:** Crea función reutilizable en [lenguaje] para [propósito].
23. **HTML/CSS:** Genera fragmento para [componente] con diseño moderno y accesible.
24. **Migración DB:** Script (pseudocódigo/ORM) para añadir columna [nombre] tipo [tipo] a tabla [tabla].
25. **Plantilla Email:** Crea HTML transaccional para [contexto] responsive y con placeholders.

### 3. Garantía de Calidad y Mantenimiento de Código (26-40)
26. **Code Review:** Actúa como senior meticuloso. Revisa [código] analizando SOLID, bugs, optimización, legibilidad y seguridad.
27. **Refactorizar legibilidad:** Refactoriza [código] aplicando Clean Code (nombres claros, extraer métodos, simplificar condicionales).
28. **Encontrar bugs:** Analiza [función] en busca de errores lógicos, explica el bug y da versión corregida.
29. **Optimizar función lenta:** Identifica cuellos de botella en [código] y proporciona versión optimizada explicando mejoras.
30. **Generar tests unitarios:** Actúa como QA. Genera suite en [framework] cubriendo happy path, casos edge y errores.
31. **Tests de integración:** Plan de tests para [Módulo A] y [Módulo B] con 3 casos en pseudocódigo.
32. **Documentar existente:** Genera comentarios de documentación detallando fin, parámetros y retornos de [función].
33. **Mejorar errores:** Reemplaza try-catch genéricos por excepciones específicas y mensajes informativos en [código].
34. **Vulnerabilidades:** Actúa como experto seguridad. Analiza [código] buscando inyección, XSS, etc. y da mitigación.
35. **Estilo funcional:** Reescribe [código] imperativo usando map/filter/reduce y evitando mutación.
36. **Aplicar patrón:** Refactoriza [código] para usar el patrón [patrón] detallando beneficios.
37. **Verificar guías:** Señala desviaciones de [guía de estilo] en el siguiente fragmento: [código].
38. **Simplificar condicionales:** Transforma if-else anidados en guard clauses o tablas de búsqueda.
39. **Principio DRY:** Identifica lógica duplicada en [fragmento A] y [fragmento B] y extráela a función.
40. **Logging:** Añade sentencias de log informativas (eventos, warnings, errores) a [código].

### 4. Planificación Estratégica y Arquitectura de Software (41-45)
41. **Proponer patrón:** Actúa como arquitecto. Sugiere 2-3 patrones para el problema [problema] con pros/contras.
42. **Estructura JSON:** Diseña JSON para representar [objeto] con campos anidados y diversos tipos de datos.
43. **Arquitectura Microservicio:** Esboza arquitectura para servicio de [fin] con endpoints, dependencias y stack.
44. **Plan de feature:** Actúa como gestor técnico. Desglosa [funcionalidad] en tareas pequeñas ordenadas por dependencia.
45. **Estructura proyecto:** Sugiere directorios estándar y escalables para proyecto [tipo] explicando cada carpeta.

### 5. Herramientas de Desarrollo y Línea de Comandos (46-50)
46. **Git complejo:** Actúa como experto. Comando único para [objetivo] con explicación de cada flag.
47. **Script de Shell:** Escribe script [Bash/PowerShell] que automatice [tarea].
48. **Comando AWK/SED:** One-liner para procesar archivo [acción].
49. **Pipeline CI/CD:** Configuración básica para [plataforma] (dependencias -> tests -> build).
50. **Explicar CLI:** Desglosa comando [comando] explicando binario, flags y argumentos.

---

## 🚀 Directrices Avanzadas para Casos de Uso Complejos
*Enfoque avanzado y detallado para tareas complejas.*

### 1. Depuración Sistemática y Análisis de Causa Raíz (1-8)
1. **Sistemático:** Experto en depuración. Califica [código], lista problemas, da pasos para depurar y explica error.
2. **Contexto específico:** Senior 20+ años. Analiza [lenguaje/framework] y error en línea [n] para dar diagnóstico raíz y prevención.
3. **Reactivo:** Detecta y corrige errores off-by-one o condiciones límite en [archivo].
4. **Rendimiento:** Identifica cuellos de botella, analiza complejidad y propone optimizaciones de memoria/CPU.
5. **Dependencias:** Analiza conflictos en [package.json/requirements.txt], sugiere versiones compatibles y mejores prácticas.
6. **Lógica de negocio:** Encuentra discrepancia entre [comportamiento esperado] y [actual] en [código].
7. **Concurrencia:** Revisa problemas de race conditions y sugiere soluciones thread-safe.
8. **Logs estratégicos:** Añade trazas con niveles adecuados e información contextual para rastrear flujo.

### 2. Refactorización Estructural y Optimización de Código (9-16)
9. **Multi-step:** Proceso completo: estándares modernos -> corregir lógica -> revisión final -> crear tests.
10. **Arquitecto de optimización:** Optimiza [código] para reducir complejidad y mejorar memoria sin perder legibilidad.
11. **Modular:** Divide funciones grandes en pequeñas de responsabilidad única en [módulo].
12. **Modernización:** Migra APIs deprecadas y convierte callbacks a async/await en [archivo].
13. **Código muerto:** Identifica y elimina código no utilizado analizando impacto.
14. **Semántico:** Renombra variables/funciones en [archivo] según convenciones del lenguaje.
15. **Patrones:** Convierte [código] al patrón [específico] detallando beneficios.
16. **Estructura datos:** Optimiza estructuras para velocidad de acceso y bajo consumo de memoria.

### 3. Ecosistemas de Testing y Control de Calidad Avanzado (17-24)
17. **Suite completa:** Genera tests unitarios, edge cases, integración y rendimiento para [función] en [framework].
18. **Cobertura:** Identifica gaps en cobertura basado en [requisitos] y sugiere tests de riesgo/impacto.
19. **Automatización:** Genera tests con setup/teardown y mocks estratégicos para [función].
20. **Integración pro:** Validación de contratos entre servicios y flujo de datos.
21. **End-to-End:** Escribe tests E2E para [user journey] incluyendo casos de error.
22. **Mocking estratégico:** Aísla tests mockeando servicios externos con respuestas realistas.
23. **Casos límite:** Testea inputs inválidos, nulos y límites de memoria en [función].
24. **Property-based:** Genera tests basados en propiedades (fuzzing) para validar invariantes.

### 4. Documentación Técnica y Comunicación de Arquitectura (25-30)
25. **Módulos:** Documenta propósito, responsabilidades, ejemplos y diagramas de [módulo].
26. **No-técnicos:** Explica [código] para perfiles de negocio detallando qué hace y resultados esperados.
27. **Auto-API:** Genera doc de endpoints, parámetros, respuestas y errores para [módulo].
28. **Docstrings Google:** Aplica estilo Google (params, returns, raises) a métodos de [clase].
29. **README Pro:** Genera README con badges, screenshots, roadmap e instrucciones claras.
30. **Arquitectura:** Resumen de alto nivel con diagramas C4 y registro de decisiones (ADR).

### 5. Metodologías de Aprendizaje y Transferencia de Conocimiento (31-36)
31. **Línea por línea:** Explica qué hace, por qué se usa ese patrón y alternativas para [código].
32. **Proyectos de práctica:** Sugiere proyecto nivel [rango] con hitos, stack y estructura para practicar [tema].
33. **Instructor:** Explica [concepto] con ejemplos simples, casos reales y ejercicios progresivos.
34. **Analogías:** Paralelismos entre [concepto técnico complejo] y situaciones cotidianas.
35. **Trade-offs:** Compara enfoques para [problema] detallando pros/contras y complejidad.
36. **Tutoriales:** Guía paso a paso verificable con outputs esperados para [funcionalidad].

### 6. Auditoría y Revisión Crítica de Código (37-44)
37. **Empresarial:** Analiza SOLID, seguridad OWASP, nomenclatura y escalabilidad en [código].
38. **Aspecto específico:** Revisa enfocándote solo en [rendimiento/seguridad/estilo].
39. **Seguridad profunda:** Escaneo de vulnerabilidades, algoritmos obsoletos y autorización.
40. **Mejores prácticas:** Evalúa adherencia a estándares de [tecnología] y da puntaje.
41. **Arquitectura:** Revisa patrones, acoplamiento y separación de responsabilidades.
42. **Rendimiento:** Audita operaciones O(n²), memory leaks y blocking operations.
43. **Mantenibilidad:** Evalúa complejidad ciclomática y cohesión.
44. **Testing:** Revisa si los tests son determinísticos y siguen el patrón AAA.

### 7. Generación Sintética y Diseño de Arquitecturas (45-50)
45. **Contexto específico:** Genera función con lógica de negocio, validación y tests para [dominio].
46. **Especificaciones:** Redacta especificación técnica (arquitectura, APIs, riesgos) para [sistema].
47. **Sistemas distribuidos:** Sugiere patrones para escalabilidad, asincronía y alta disponibilidad.
48. **Diagramas Mermaid:** Genera código Mermaid para diagramas de arquitectura (C4).
49. **Boilerplate inteligente:** Crea estructura base con validaciones, tipos y carpetas para [tecnología].
50. **Análisis de trade-offs:** Evaluación de fortalezas, riesgos y plan de evolución para [diseño].

---

## 💡 Mejores Prácticas para la Ingeniería de Prompts
1. **Define tu objetivo primero:** Articula con precisión el resultado deseado.
2. **Especificidad vence a la ambigüedad:** Incluye todos los detalles relevantes.
3. **Divide y vencerás:** Descompón problemas complejos en subtareas pequeñas.
4. **Itera y refina:** La interacción es una conversación, no un comando único.
5. **Usa instrucciones positivas:** Di qué hacer en lugar de qué no hacer (cuando sea posible).
6. **"Piensa paso a paso":** Incita a la IA a usar *Chain-of-Thought* para razonamientos lógicos.
7. **Verifica siempre:** Audita el código generado; la IA puede "alucinar" detalles técnicos.
8. **Crea tu biblioteca personal:** Guarda los prompts que mejor te funcionen.
9. **Entiende las limitaciones:** Recuerda que los LLMs tienen una fecha de corte de conocimiento.
10. **Experimenta constantemente:** La mejor forma de aprender es probando diferentes enfoques.

## 🏁 Checklist de Calidad RAG
- [x] Frontmatter completo con metadatos de navegación.
- [x] TL;DR presente y conciso.
- [x] Títulos descriptivos para cada sección.
- [x] Enlaces semánticos con etiquetas de relación.
- [x] Estructura jerárquica clara y secciones autónomas.
