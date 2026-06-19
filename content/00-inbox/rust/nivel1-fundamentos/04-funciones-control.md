# Funciones y Control de Flujo

## Funciones en Rust

Las funciones son bloques de código reutilizables que realizan una tarea específica. En Rust, las funciones se declaran usando la palabra clave `fn`.

### Sintaxis básica

```rust
fn nombre_funcion(parametro1: Tipo1, parametro2: Tipo2) -> TipoDeRetorno {
    // Cuerpo de la función
    // expresión final (sin punto y coma) para retorno implícito
    // o usar return explícitamente
}
```

### Ejemplos de funciones

```rust
// Función sin parámetros y sin retorno
fn saludo() {
    println!("¡Hola, mundo!");
}

// Función con parámetros y retorno explícito
fn suma(a: i32, b: i32) -> i32 {
    return a + b;
}

// Función con retorno implícito (más idiomática en Rust)
fn suma_implicita(a: i32, b: i32) -> i32 {
    a + b  // Nota: sin punto y coma
}

// Función que retorna tupla
fn dividir(dividendo: i32, divisor: i32) -> (i32, i32) {
    let cociente = dividendo / divisor;
    let resto = dividendo % divisor;
    (cociente, resto)
}

// Función genérica (veremos más detalles en nivel 2)
fn dup<T: Clone>(x: T) -> (T, T) {
    (x.clone(), x.clone())
}
```

### Llamada a funciones

```rust
fn main() {
    saludo();                           // Llamada simple
    
    let resultado = suma(5, 3);        // Con retorno explícito
    println!("5 + 3 = {}", resultado);
    
    let resultado2 = suma_implicita(10, 4); // Con retorno implícito
    println!("10 + 4 = {}", resultado2);
    
    let (coc, resto) = dividir(17, 5);
    println!("17 / 5 = cociente {}, resto {}", coc, resto);
}
```

## Expresiones vs Statements

Una distinción importante en Rust es entre expresiones y statements:

- **Statements**: Instrucciones que realizan una acción pero no retornan un valor (termina en `;`)
- **Expresiones**: Evaluan a un valor (no terminan en `;`, excepto cuando se usan como parte de un statement)

```rust
// Estos son statements:
let y = 6;
if x > 0 { println!("positivo"); }

// Estos son expresiones:
x + y
if x > 0 { x } else { -x }  // Esta expresión retorna un valor
x += 1;  // Esto es un statement porque += retorna ()
```

### ¿Por qué importa esta distinción?

Porque el cuerpo de una función debe terminar en una expresión (no un statement) si queremos retornar un valor:

```rust
fn cinco() -> i32 {
    5  // Esto es una expresión, retorna 5
}

fn seis() -> i32 {
    let x = 5;  // Esto es un statement
    x + 1       // Esto es una expresión, retorna 6
}

fn siete() -> i32 {
    let x = 5;
    x + 1;  // ¡ERROR! Esto es un statement, no retorna nada
    // Necesitamos quitar el punto y coma o usar return explícito
}
```

## Control de Flujo

### Sentencia if

```rust
let numero = 7;

if numero < 5 {
    println!("condición verdadera");
} else if numero == 5 {
    println!("exactamente cinco");
} else {
    println!("condición falsa");
}

// if como expresión (muy útil)
let condicion = true;
let valor = if condicion { 5 } else { 6 }; // valor será 5
// Tipo debe ser el mismo en todas las ramas
// let valor = if condicion { 5 } else { "seis" }; // ERROR: tipos diferentes
```

### Bucles

#### Loop infinito

```rust
let mut contador = 0;

loop {
    contador += 1;
    if contador == 10 {
        break;  // Sale del bucle
    }
    
    if contador % 2 == 0 {
        continue;  // Salta al siguiente iteration
    }
    
    println!("valor impar: {}", contador);
}

// Loop con etiqueta para bucles anidados
'contador_externo: loop {
    let mut contador_interno = 0;
    loop {
        contador_interno += 1;
        if contador_interno == 5 {
            break 'contador_externo;  // Sale del bucle externo
        }
        if contador_interno == 2 {
            continue;  // Continúa el bucle interno
        }
        println!("interno: {}", contador_interno);
    }
}
```

#### Bucle while

```rust
let mut pila = vec![1, 2, 3, 4, 5];

while let Some(valor) = pila.pop() {
    println!("Valor desapilado: {}", valor);
}

// Equivalente con match explícito
let mut pila = vec![1, 2, 3, 4, 5];
while !pila.is_empty() {
    match pila.pop() {
        Some(valor) => println!("Valor: {}", valor),
        None => break,  // Nunca sucederá pero es requerido por el compilador
    }
}
```

#### Bucle for

```rust
// Iterando sobre rangos
for i in 0..5 {
    println!("{}", i);  // Imprime 0,1,2,3,4
}

for i in 0..=5 {
    println!("{}", i);  // Imprime 0,1,2,3,4,5
}

// Iterando sobre colecciones
let numeros = vec![10, 20, 30, 40, 50];
for num in &numeros {
    println!("Valor: {}", num);
}

// Con índices usando enumerate
for (indice, valor) in numeros.iter().enumerate() {
    println!("{}: {}", indice, valor);
}

// Iterando sobre arrays fijos
let pares = [2, 4, 6, 8, 10];
for num in pares {
    println!("Par: {}", num);
}

// Iterando con referencia para no tomar ownership
let textos = vec!["uno", "dos", "tres"];
for texto in &textos {
    println!("Texto: {}", texto);
    // textos sigue siendo usable después del bucle
}
```

### Match - Comparación por patrones

Rust tiene una poderosa construcción `match` que es como un switch mejorado:

```rust
enum EstadoTrafico {
    Rojo,
    Amarillo,
    Verde,
}

fn interpretar_senal(estado: EstadoTrafico) {
    match estado {
        EstadoTrafico::Rojo => println!("Detente"),
        EstadoTrafico::Amarillo => println!("Precaución"),
        EstadoTrafico::Verde => println::println!("Avanza"),
    }
}

// Con valores asociados
enum MensajeWeb {
    Get { ruta: String },
    Post { ruta: String, cuerpo: String },
    Put { ruta: String },
    Delete,
}

fn procesar_solicitud(msg: MensajeWeb) {
    match msg {
        MensajeWeb::Get { ruta } => {
            println!("GET request para {}", ruta);
        }
        MensajeWeb::Post { ruta, cuerpo } => {
            println!("POST request para {} con cuerpo: {}", ruta, cuerpo);
        }
        MensajeWeb::Put { ruta } => {
            println!("PUT request para {}", ruta);
        }
        MensajeWeb::Delete => {
            println!("DELETE request");
        }
    }
}

// Con rangos y condiciones
fn clasificacion_edad(edad: u8) -> &'static str {
    match edad {
        0..=12 => "niño",
        13..=19 => "adolescente",
        20..=64 => "adulto",
        65..=100 => "anciano",
        _ => "edad inválida",  // _ es el patrón de comodín
    }
}

// Match como expresión (retorna un valor)
let numero = 7;
let es_par = match numero % 2 {
    0 => true,
    _ => false,
};
```

### If let - Sintaxis concisa para un solo caso

Cuando solo te importa un patrón específico en lugar de todos los posibles:

```rust
let algunos_numeros = vec![-2, 3, -5, 7, -11, 13];

// Sin if let (más verboso)
for numero in algunos_numeros {
    match numero {
        // Solo nos interesan los números positivos
        x if x > 0 => println!("{} es positivo", x),
        _ => (),  // Hacemos nada con otros casos
    }
}

// Con if let (más conciso)
for numero in algunos_numeros {
    if let x = numero {
        if x > 0 {
            println!("{} es positivo", x);
        }
    }
}

// Mejor aún: if let con patrón
for numero in algunos_numeros {
    if let x = numero {
        if x > 0 {
            println!("{} es positivo", x);
        }
    }
}

// O aún más directo con if let y condición
for numero in algunos_numeros {
    if let x = numero {
        if x > 0 {
            println!("{} es positivo", x);
        }
    }
}

// La forma más limpia
for numero in algunos_numeros {
    if numero > 0 {
        println!("{} es positivo", numero);
    }
}

// Pero if let realmente brilla con enums y opciones
let opcion_algo: Option<i32> = Some(7);

match opcion_algo {
    Some(i) => println!("Encontrado: {}", i),
    None => println!("Nada encontrado"),
}

// Equivalente con if let (menos código cuando solo nos importa un caso)
if let Some(i) = opcion_algo {
    println!("Encontrado: {}", i);
}
// No necesitamos manejar el caso None explícitamente
```

## Funciones con múltiples retornos usando tuplas

Ya vimos un ejemplo, pero profundicemos más:

```rust
// Devolver múltiples valores
fn calcularEstadisticas(numeros: &[f64]) -> (f64, f64, f64) {
    let suma: f64 = numeros.iter().sum();
    let promedio = suma / numeros.len() as f64;
    
    let mut ordenados = numeros.to_vec();
    ordenados.sort_by(|a, b| a.partial_cmp(b).unwrap());
    let mediana = if ordenados.len() % 2 == 0 {
        (ordenados[ordenados.len()/2 - 1] + ordenados[ordenados.len()/2]) / 2.0
    } else {
        ordenados[ordenados.len()/2]
    };
    
    let varianza: f64 = numeros.iter()
        .map(|x| (x - promedio).powi(2))
        .sum::<f64>() / numeros.len() as f64;
    let desviacion_estandar = varianza.sqrt();
    
    (promedio, mediana, desviacion_estandar)
}

// Uso
fn main() {
    let datos = [1.0, 2.0, 3.0, 4.0, 5.0];
    let (prom, mediana, std) = calcularEstadisticas(&datos);
    
    println!("Promedio: {}", prom);
    println!("Mediana: {}", mediana);
    println!("Desviación estándar: {}", std);
}
```

## Early returns y manejo de errores básico

```rust
fn dividir_seguro(dividendo: f64, divisor: f64) -> Option<f64> {
    if divisor == 0.0 {
        return None;  // Early return
    }
    Some(dividendo / divisor)
}

// Con Result para más información sobre errores
fn dividir_result(dividendo: f64, divisor: f64) -> Result<f64, String> {
    if divisor == 0.0 {
        Err("División por cero".to_string())
    } else {
        Ok(dividendo / divisor)
    }
}

// Uso con match
fn main() {
    match dividir_result(10.0, 0.0) {
        Ok(resultado) => println!("Resultado: {}", resultado),
        Err(error) => println!("Error: {}", error),
    }
    
    // O con if let para éxito
    if let Ok(resultado) = dividir_result(10.0, 2.0) {
        println!("Éxito: {}", resultado);
    }
}
```

## Ejercicios

1. Implementa una función que calcule el factorial de un número usando tanto iteración como recursión.
2. Crea una función que determine si un año es bisiesto.
3. Implementa el juego de Piedra, Papel o Tijera donde la computadora elige aleatoriamente y tú juegas contra ella.
4. Escribe una función que convierta un número romano a decimal (por ejemplo, XIV → 14).
5. Implementa una función que encuentre el máximo común divisor (MCD) de dos números usando el algoritmo de Euclides.
6. Crea un programa que imprima la tabla de multiplicar de un número dado usando bucles anidados.