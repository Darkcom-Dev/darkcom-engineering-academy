---
tema: "Funciones"
leccion: 05
titulo: "Math"
---

# Math

**Módulo:** `Funciones` | **Lección:** 05

## 🧩 Código JavaScript

```javascript
function potenciar(base, exponente){
                return Math.pow(base, exponente)
            }

            function calcularCircunferencia(diametro){
                return Math.PI * diametro;
            }
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
- [[Return]]
- [[Parámetros]]
- [[Calculador de Combustible]]
- [[Dibujando en Canvas]]
- [[Random]]
- [[Calculadora]]
- [[Calculadora]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Variables]]
- ➡️ Módulo siguiente: [[Flujo del Programa]]
- 🏠 Volver al [[Índice del Curso]]
