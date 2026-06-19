---
tema: "Funciones"
leccion: 03
titulo: "Parámetros"
---

# Parámetros

**Módulo:** `Funciones` | **Lección:** 03

Primer Número

🔹 **Campo** `number`: 

Segundo Número

🔹 **Campo** `number`: 

🔹 **Campo** `text`: Calcular

## 🧩 Código JavaScript

```javascript
function sumar(numero1, numero2){
                resultado = +numero1 + +numero2;
                return resultado;
            }

            function mostrarResultado(){
                let elementoNumero1 = document.getElementById("primerNumero");
                let elementoNumero2 = document.getElementById("segundoNumero");
                let elementoTexto = document.getElementById("textoResultado");
                let elementoSuma = sumar(elementoNumero1.value, elementoNumero2.value);
            
                elementoTexto.textContent = elementoSuma;
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
