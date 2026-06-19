# Lección 02: Consideraciones y Arquitectura (ExpressJS)

Para construir aplicaciones modernas en JavaScript, solemos utilizar el entorno de ejecución **Node.js** junto con un marco de trabajo llamado **Express.js** para crear el servidor.

## ¿Qué es Express.js?
Es un framework minimalista para Node.js que simplifica la creación de servidores web y APIs. Proporciona herramientas para manejar rutas, peticiones y respuestas de forma sencilla.

### Ejemplo de Servidor Básico
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('¡Hola desde el Servidor!');
});

app.listen(3000, () => {
    console.log('Servidor corriendo en el puerto 3000');
});
```

## Middleware: La Capa Intermedia
Express utiliza **Middlewares**, que son funciones que se ejecutan entre que llega la petición y se envía la respuesta. Se usan para:
- Autenticación (verificar si el usuario está logueado).
- Parseo de datos (convertir el cuerpo de la petición a JSON).
- Logs (registrar qué está pasando en el servidor).

## Tecnologías Similares
Si vienes de otros lenguajes o buscas alternativas, estas son las opciones equivalentes:

| Lenguaje | Framework Similar |
| :--- | :--- |
| **JavaScript** | Koa.js, Fastify, NestJS |
| **Python** | Flask, FastAPI |
| **PHP** | Laravel (Lumen para APIs) |
| **Ruby** | Sinatra |

## ¿Qué es GraphQL?
Aunque en este curso nos enfocamos en **REST**, existe otra tecnología llamada **GraphQL**. A diferencia de REST, donde tienes muchas URLs para diferentes datos, en GraphQL hay una sola URL y tú pides exactamente los campos que necesitas, optimizando el ancho de banda.

---
[[16-01-introduccion-db|<- Anterior]] | [[00-indice-curso|Índice]] | [[17-01-proyecto-final|Siguiente ->]]
