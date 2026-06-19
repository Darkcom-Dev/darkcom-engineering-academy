---
tema: "Funciones"
leccion: 02
titulo: "Return"
---

# Return

**Módulo:** `Funciones` | **Lección:** 02

## 🧩 Código JavaScript

```javascript
function sumar(){
                resultado = 2 + 3;
                return resultado;
                nombre = "Juan";
                return nombre
            }
            
            let numero = "El numero es " + sumar();
            alert(nombre);
```


## 📊 Diagrama Conceptual

```mermaid
flowchart TD
    A[Función] --> B[Declaración: function f(){}]
    A --> C[Expresión: const f = function(){}]
    A --> D[Arrow: const f = () => {}]
    B --> E[Hosting]
    C --> F[No hosting]
    D --> G[this léxico]
    B --> H[Usar return]
    C --> H
```



## 🔗 Enlaces Relacionados

### En este módulo
- [[Funciones]]
- [[Parámetros]]
- [[Calculador de Combustible]]
- [[Dibujando en Canvas]]
- [[Math]]
- [[Random]]
- [[Calculadora]]
- [[Calculadora]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Variables]]
- ➡️ Módulo siguiente: [[Flujo del Programa]]
- 🏠 Volver al [[Índice del Curso]]
