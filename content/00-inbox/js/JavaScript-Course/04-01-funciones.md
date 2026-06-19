# Lección 01: Concepto de Función

Las funciones son bloques de código diseñados para realizar una tarea específica. Son los "verbos" de nuestro programa: acciones que podemos invocar cuando las necesitemos.

## ¿Por qué usar funciones?
1. **Reutilización:** Escribes el código una vez y lo usas muchas.
2. **Organización:** Dividen problemas grandes en pequeñas tareas.
3. **Mantenimiento:** Si hay un error, solo lo arreglas en un lugar.

### Estructura Básica
```javascript
function nombreDeLaFuncion() {
    // Código a ejecutar
}
```

### Ejemplo
```javascript
function sumar() {
    let resultado = 2 + 3;
    console.log(resultado);
}

// Para que el código se ejecute, debemos INVOCAR la función
sumar();
```

---
[[03-09-proyecto-dia-3|<- Anterior]] | [[00-indice-curso|Índice]] | [[04-02-return|Siguiente: La sentencia Return ->]]
