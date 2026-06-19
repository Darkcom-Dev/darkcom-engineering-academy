# Lección 02: Uso Estricto (use strict)

El **Modo Estricto** es una funcionalidad introducida en ECMAScript 5 que permite cambiar el comportamiento de JavaScript para que sea más riguroso y lance errores en situaciones donde normalmente fallaría silenciosamente.

## ¿Cómo activar el Modo Estricto?
Se activa escribiendo la cadena `"use strict";` al principio de un archivo o de una función.

```javascript
"use strict";

// Esto lanzaría un error porque la variable no está declarada con let/const
miVariable = 10; 
```

## ¿Qué errores previene?

1. **Variables no declaradas:** Impide el uso de variables globales accidentales.
2. **Parámetros duplicados:** Lanza error si una función tiene dos argumentos con el mismo nombre.
3. **Eliminación de propiedades no borrables:** Evita intentar borrar cosas que el lenguaje protege.
4. **Palabras reservadas:** Protege nombres que podrían usarse en futuras versiones de JS (ej: `private`, `interface`).

### Ejemplo Comparativo
```javascript
// Sin modo estricto (Peligroso)
x = 3.14; // Funciona, pero crea una global accidental

// Con modo estricto (Seguro)
"use strict";
y = 3.14; // Uncaught ReferenceError: y is not defined
```

## Recomendación Profesional
Hoy en día, la mayoría de los desarrolladores trabajan siempre en modo estricto. Además, **todos los módulos de JavaScript (`type="module"`) operan en modo estricto por defecto.**

---
[[18-01-modulos|<- Anterior]] | [[00-indice-curso|Índice]] | Fin del Curso
