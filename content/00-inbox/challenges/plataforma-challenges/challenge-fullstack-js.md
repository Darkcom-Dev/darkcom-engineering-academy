---
tipo: desafio
dificultad: 5
---

# 🌐 Challenge Full Stack — Administración de Presupuesto Personal

---

## 🎯 Objetivo

Desarrollar una aplicación para **administración de presupuesto personal**. Debe permitir crear y editar **ingresos y egresos** de dinero, y mostrar un **balance resultante**.

---

## 📚 Conocimientos Previos

- Conceptos de arquitectura de aplicaciones: capas (presentación, lógica de negocio, datos)
- Principios de separación de responsabilidades y acoplamiento bajo
- Modelado de datos y diseño de esquemas para entidades financieras
- Mecanismos de persistencia: operaciones CRUD, transacciones, integridad referencial
- Principios de comunicación entre sistemas: solicitudes-respuesta, formatos de datos (JSON/XML)
- Conceptos de autenticación y autorización básica
- Principios de diseño de interfaces de usuario: usabilidad, retroalimentación, accesibilidad
- Técnicas de manejo de estado en aplicaciones interactivas
- Estrategias de validación de entrada y reglas de negocio

---

## 🧩 Funcionalidades

### Dashboard Principal
- Visualización del balance actual (ingresos menos egresos)
- Lista de movimientos recientes con capacidad de filtrado por tipo
- Acciones para crear nuevos ingresos y egresos

### Gestión de Operaciones Financieras
- Creación de nuevas operaciones con: concepto, monto, fecha y tipo (ingreso/egreso)
- Edición de operaciones existentes (excepto el tipo, que permanece inmutable tras creación)
- Eliminación de operaciones del registro
- Filtrado y visualización de operaciones por tipo (ingreso o egreso)

### Categorización (Opcional)
- Asignación de categorías a operaciones para mejor organización
- Visualización de operaciones agrupadas por categoría
- Reportes de gastos e ingresos por categoría

### Experiencia de Usuario
- Interfaz clara y responsive para diferentes tamaños de pantalla
- Validación en tiempo real de datos de entrada
- Feedback visual durante operaciones de guardado y eliminación
- Confirmación antes de acciones destructivas (eliminación)

---

## 💡 Sugerencias de Investigación

Para resolver este desafío, se recomienda investigar sobre:

- Patrones de arquitectura de aplicaciones: Capas, Hexagonal, Clean Architecture
- Técnicas de modelado de datos financieros: normalización, manejo de períodos fiscales
- Estrategias de manejo de estado: Flux, Redux, Context API, estado local
- Principios de diseño de APIs RESTful: recursos, verbos, códigos de estado
- Mecanismos de validación de datos: esquemas, validación en cliente y servidor
- Técnicas de persistencia: ORM vs mapeo directo, estrategias de caché
- Principios de diseño de interfaces: material design, diseño atómico, sistemas de componentes
- Estrategias de testing: unitario, integración, end-to-end para aplicaciones full-stack
- Técnicas de deployment y despliegue continuo: containers, CI/CD
- Principios de seguridad básica: sanitización de entrada, protección contra inyecciones
- Marcos de trabajo para gráficos y visualización de datos: Chart.js, D3, Recharts
- Técnicas de internacionalización y localización (i18n/l10n) para aplicaciones financieras

---

## 📖 Documentación

Documentar la aplicación incluyendo:
- Arquitectura de la solución y flujo de datos entre capas
- Modelo de datos y relaciones entre entidades
- Guía de instalación y configuración
- Manual de uso para usuarios finales
- Detalles de endpoints de API (si aplica) y formatos de intercambio
- Instrucciones de pruebas y validación

---

## 🧪 Tests (Opcional)

Agregar pruebas para verificar:
- Creación, lectura, actualización y eliminación correcta de operaciones
- Cálculo preciso del balance y filtros por tipo
- Validación adecuada de datos de entrada (tipos, rangos, formatos)
- Funcionalidad de categorización (si se implementa)
- Interfaz de usuario: elementos interactivos, estados visuales, accesibilidad
- Responsive design: comportamiento en diferentes tamaños de pantalla
- Manejo de errores: casos de falla y recuperación apropiada

---

## ✅ Criterios de evaluación

| Criterio | Descripción |
|----------|-------------|
| 🏗️ Arquitectura | Diseño modular con separación clara de responsabilidades |
| 📊 Funcionalidad | Implementación completa de todas las características requeridas |
| 🧹 Calidad de Código | Código limpio, legible y siguiendo buenas prácticas de programación |
| 🔄 Estado | Manejo adecuado y predecible del estado de la aplicación |
| 💾 Persistencia | Almacenamiento confiable y consistente de los datos financieros |
| 🖥️ Interfaz de Usuario | Experiencia de usuario intuitiva y accesible |
| 🔐 Seguridad | Medidas básicas de protección contra vulnerabilidades comunes |
| 🧪 Testing | Cobertura adecuada de pruebas automatizadas (si se implementa) |
| 📱 Responsividad | Adaptación correcta a diferentes dispositivos y tamaños de pantalla |
| 📈 Escalabilidad | Diseño que permite crecimiento futuro en funcionalidades y usuarios |

---

## 🔗 Challenges y retos similares
- [[plataforma-challenges/challenge-backend-node]] — Backend Node
- [[plataforma-challenges/challenge-frontend-angular]] — Frontend Angular
- [[plataforma-challenges/challenge-data-analytics]] — Data Analytics Python
- [[retos/reto-gestion-pedidos]] — MVC empresarial
- [[retos/05-calculadora-financiera]] — MVC financiero
- [[retos/04-reportes]] — Reportes financieros
- [[retos/reto-java-jdbc]] — CRUD completo