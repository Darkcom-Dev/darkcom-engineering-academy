---
tipo: desafio
dificultad: 5
---

# 🅰️ Challenge Frontend — Menú Virtual para Restaurante

---

## 🎯 Objetivo

Desarrollar una aplicación para crear una **carta de opciones de menús** para el restaurante «A la carta». La app consumirá una API externa y mostrará atributos de cada plato y del menú finalizado.

---

## 📚 Conocimientos Previos

- Conceptos de desarrollo frontend: componentes, estado, props y eventos
- Arquitecturas de aplicaciones frontend: MVC, MVVM, Flux, Redux
- Consumo de APIs RESTful: métodos HTTP, códigos de estado, headers
- Mecanismos de autenticación en aplicaciones frontend: tokens, cookies, sesiones
- Principios de diseño responsive y accesibilidad web (WCAG)
- Técnicas de manejo de formularios: validación, manipulación de estado
- Conceptos de reactivity y programación reactiva: observables, streams
- Estrategias de optimización de rendimiento: lazy loading, memoization, caching

---

## 🧩 Funcionalidades

### Autenticación de Usuarios
- Sistema de login con validación de credenciales
- Almacenamiento seguro de tokens de autenticación
- Protección de rutas basada en estado de autenticación
- Feedback visual durante procesos de autenticación

### Gestión del Menú
- Visualización de lista de platos disponibles desde API externa
- Búsqueda y filtrado de platos por nombre o características
- Selección de platos para crear un menú personalizado
- Validación de reglas de composición del menú (cantidad, tipos de platos)
- Eliminación de platos del menú con recálculo automático de totales

### Visualización de Información
- Cálculo y display de acumulativos: precio total del menú
- Cálculo y display de promedios: tiempo de preparación, health score
- Presentación detallada de platos seleccionados
- Indicadores visuales de estado y progreso

### Experiencia de Usuario
- Diseño responsive para múltiples tamaños de pantalla
- Transiciones suaves y feedback visual en interacciones
- Manejo apropiado de estados de carga y error
- Navegación intuitiva entre vistas de la aplicación
- Accesibilidad: contraste adecuado, navegación por teclado, labels descriptivos

---

## 💡 Sugerencias de Investigación

Para resolver este desafío, se recomienda investigar sobre:

- Arquitecturas frontend modernas: Micro-frontends, Island Architecture
- Patrones de manejo de estado: Redux Toolkit, Zustand, Jotai, Recoil
- Técnicas de data fetching: SWR, React Query, TanStack Query
- Estrategias de optimización de bundling: code splitting, tree shaking
- Principios de diseño de sistemas: Atomic Design, Design Tokens
- Marcos de trabajo para testing frontend: Jest, Vitest, Testing Library
- Técnicas de internacionalización (i18n) y localización (l10n)
- Estrategias de manejo de formularios complejos: validación dinámica, campos dependientes
- Patrones de componentes reutilizables: Compound Components, Render Props
- Técnicas de animación y motion design: Framer Motion, GSAP
- Mejores prácticas en seguridad frontend: XSS prevention, CSRF protection, CORS

---

## 📖 Documentación

Documentar la aplicación incluyendo:
- Arquitectura de componentes y flujo de datos
- Guía de instalación y configuración
- Manual de uso para usuarios finales
- Detalles de consumo de la API externa (endpoints, parámetros, autenticación)
- Instrucciones de deployment y escalabilidad

---

## 🧪 Tests (Opcional)

Agregar pruebas para verificar:
- Funcionalidad de autenticación: login, logout, protección de rutas
- Consumo correcto de API externa: manejo de estados de carga, éxito y error
- Validación de reglas de menú: composición, cantidades, restricciones
- Cálculo preciso de acumulativos y promedios
- Interfaz de usuario: elementos interactivos, estados visuales, accesibilidad
- Responsive design: comportamiento en diferentes tamaños de pantalla

---

## ✅ Criterios de evaluación

| Criterio | Descripción |
|----------|-------------|
| 🔐 Autenticación | Implementación segura de login y gestión de sesión |
| 📡 Integración API | Consumo eficaz y manejo apropiado de APIs externas |
| 🗃️ Manejo de Estado | Arquitectura clara y eficiente para el estado de la aplicación |
| 🔄 Feedback Usuario | Respuestas visuales y informativas a acciones del usuario |
| ✅ Validación | Validación robusta de formularios y reglas de negocio |
| 🔄 Componentes | Comunicación efectiva entre componentes (input/output, eventos) |
| 📋 Renderizado | Visualización eficiente y correcta de listas y datos |
| 🧭 Navegación | Flujo de navegación lógico y protegido según corresponda |
| 📱 Responsividad | Adaptación adecuada a diferentes dispositivos y tamaños de pantalla |
| ⚠️ Manejo de Errores | Estrategias apropiadas para captura, reporte y recuperación de errores |

---

## 🔗 Challenges y retos similares
- [[plataforma-challenges/challenge-backend-node]] — Backend Node
- [[plataforma-challenges/challenge-fullstack-js]] — Fullstack JS
- [[plataforma-challenges/challenge-data-analytics]] — Data Analytics Python
- [[retos/reto-formulario-transporte]] — Formularios Java
- [[retos/reto-graficas-excel]] — Visualización de datos
- [[../midudev-javascript/21-tabla-de-regalos]] — Tablas y datos