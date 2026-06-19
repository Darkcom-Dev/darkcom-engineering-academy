---
tipo: desafio
dificultad: 5
---

# 🚀 Challenge Backend — API REST para Disney

---

## 🎯 Objetivo

Desarrollar una **API REST** para explorar el mundo de **Disney**. La API permitirá conocer y modificar los personajes que lo componen y entender en qué películas participaron.

> No es necesario armar el Frontend.

---

## 📚 Conocimientos Previos

- Conceptos de arquitectura REST y recursos HTTP
- Diseño de bases de datos relacionales y modelado entidad-relación
- Mecanismos de autenticación y autorización (tokens)
- Principios de diseño de APIs: versionamiento, códigos de estado, manejo de errores
- Conceptos de serialización/deserialización de datos (JSON/XML)

---

## 🧩 Funcionalidades

### Gestión de Personajes
- Crear, leer, actualizar y eliminar personajes
- Cada personaje debe tener: imagen, nombre, edad, peso e historia

### Gestión de Películas/Series
- Crear, leer, actualizar y eliminar películas/series
- Cada película/serie debe tener: imagen, título, fecha de creación y calificación (1-5)

### Gestión de Géneros
- Crear, leer, actualizar y eliminar géneros
- Cada género debe tener: nombre e imagen

### Relaciones
- Un personaje puede participar en múltiples películas/series
- Una película/serie puede tener múltiples personajes
- Un género puede clasificar múltiples películas/series
- Una película/serie puede pertenecer a múltiples géneros

### Autenticación
- Sistema de registro de usuarios
- Sistema de inicio de sesión que devuelva un token de acceso
- Protección de endpoints usando el token de acceso

### Documentación
- Documentar todos los endpoints usando Postman o Swagger/OpenAPI

### Notificaciones (Opcional)
- Enviar email de bienvenida al registrarse

---

## 💡 Sugerencias de Investigación

Para resolver este desafío, se recomienda investigar sobre:

- Patrones de diseño para capas de aplicación: Repository Pattern, Service Layer
- Arquitecturas limpias: Hexagonal Architecture, Clean Architecture
- Estrategias de versionamiento de APIs: URI versioning, Header versioning
- Mecanismos de autenticación stateless: JWT vs OAuth2 vs API Keys
- Técnicas de optimización de consultas a bases de datos: indexing, caching
- Principios de diseño de APIs RESTful: HATEOAS, content negotiation
- Estrategias de manejo de errores en APIs: Problem Details for HTTP APIs
- Herramientas de testing de APIs: contract testing, integration testing
- Patrones de diseño para manejo de dependencias: Dependency Injection
- Técnicas de documentación automática de APIs: Swagger/OpenAPI, Redoc

---

## 📖 Documentación

Documentar los endpoints usando:
- **Postman** — colección exportable
- **Swagger** — especificación OpenAPI

---

## 🧪 Tests (Opcional)

Agregar tests para verificar:
- Campos faltantes o formato inválido en el body
- Acceso a recursos inexistentes
- Autenticación requerida
- Validación de reglas de negocio

---

## ✅ Criterios de evaluación

| Criterio | Descripción |
|----------|-------------|
| 🏗️ Modelado BD | Correcto diseño de relaciones y normalización |
| 🔐 Autenticación | Mecanismo de autenticación funcionando en rutas protegidas |
| 📐 REST | Rutas siguen principios REST (recursos, verbos HTTP, códigos de estado) |
| 🧹 Código limpio | Buenas prácticas de programación, separación de preocupaciones |
| 📚 Documentación | Documentación completa y actualizada de todos los endpoints |
| 🧪 Testing | Cobertura adecuada de pruebas (si se implementa) |
| 🔒 Seguridad | Implementación adecuada de medidas de seguridad básicas |

---

## 🔗 Challenges y retos similares
- [[plataforma-challenges/challenge-frontend-angular]] — Frontend Angular
- [[plataforma-challenges/challenge-fullstack-js]] — Fullstack JS
- [[plataforma-challenges/challenge-data-analytics]] — Data Analytics Python
- [[retos/reto-java-jdbc]] — CRUD con JDBC
- [[retos/reto-mer]] — Modelado de BD
- [[retos/reto-gestion-pedidos]] — MVC empresarial
- [[../midudev-javascript/23-compilador-cpu]] — Simulación de指令