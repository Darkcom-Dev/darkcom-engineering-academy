---
tema: "Funciones"
leccion: 06
titulo: "Random"
---

# Random

**Módulo:** `Funciones` | **Lección:** 06

🔹 **Campo** `text`: Dame un Aleatorio

## Número aleatorio: ---

## 🧩 Código JavaScript

```javascript
let elementoAleatorio = document.getElementById("textoAleatorio");

            function crearAleatorio(minimo, maximo) {
                maximo = maximo + 1;
                resultado = Math.floor(Math.random() * (maximo - minimo) + minimo);
                elementoAleatorio.textContent = resultado;
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
- [[Math]]
- [[Calculadora]]
- [[Calculadora]]

### Navegación del curso
- ⬅️ Módulo anterior: [[Variables]]
- ➡️ Módulo siguiente: [[Flujo del Programa]]
- 🏠 Volver al [[Índice del Curso]]
