# Lección 01: Operadores Lógicos y Comparación

Para tomar decisiones en programación, primero debemos aprender a comparar valores. Los operadores lógicos nos devuelven un valor **Booleano** (`true` o `false`).

## Operadores de Comparación
| Operador | Significado | Ejemplo |
| --- | --- | --- |
| `==` | Igual a | `5 == "5"` (true) |
| `===` | Estrictamente igual (valor y tipo) | `5 === "5"` (false) |
| `!=` | Distinto de | `5 != 3` (true) |
| `>` / `<` | Mayor / Menor que | `10 > 5` (true) |
| `>=` / `<=` | Mayor/Menor o igual | `18 >= 18` (true) |

## Operadores Lógicos (Puertas Lógicas)
Sirven para combinar varias comparaciones.

```mermaid
graph LR
    AND[AND: &&] --- AND_DESC[Ambos deben ser true]
    OR[OR: ||] --- OR_DESC[Al menos uno debe ser true]
    NOT[NOT: !] --- NOT_DESC[Invierte el valor]
```

### Ejemplo Práctico
Supongamos una fiesta con reglas:
- **Beber:** Edad >= 18.
- **Entrar:** Edad >= 18 **AND** tiene entrada.
- **Gratis:** Edad == 20 **OR** Edad == 25.

```javascript
function calcular() {
    let edad = +document.getElementById("textoEdad").value;
    let tieneEntrada = true;

    // AND (&&)
    let puedeIngresar = (edad >= 18) && tieneEntrada;
    
    // OR (||)
    let esGratis = (edad == 20) || (edad == 25);
}
```

---
[[04-07-calculadora|<- Anterior]] | [[00-indice-curso|Índice]] | [[05-02-declaracion-if|Siguiente: Declaración If ->]]
