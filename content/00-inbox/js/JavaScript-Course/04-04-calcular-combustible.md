# Lección 04: Práctica - Calculador de Combustible

En esta práctica aplicaremos lo aprendido sobre funciones con parámetros y retorno para resolver un problema matemático simple.

## El Problema
Queremos calcular cuántos litros de combustible necesita un vehículo para recorrer una distancia, sabiendo que consume cierta cantidad por kilómetro.

### Lógica de la Función
```javascript
function calcularCombustible(kilometros, consumoPorKm) {
    return kilometros * consumoPorKm;
}
```

### Implementación en el DOM
```javascript
function calcularLitros() {
    let km = +document.getElementById("textoKm").value;
    let consumo = 0.07; // Ejemplo: 7 litros cada 100km
    
    let litrosNecesarios = calcularCombustible(km, consumo);
    
    document.getElementById("textoResultado").textContent = 
        "Necesitarás " + litrosNecesarios + " litros.";
}
```

---
[[04-03-parametros|<- Anterior]] | [[00-indice-curso|Índice]] | [[04-05-math|Siguiente: El objeto Math ->]]
