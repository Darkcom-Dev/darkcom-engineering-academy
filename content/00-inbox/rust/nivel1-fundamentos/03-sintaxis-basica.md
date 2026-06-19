# Sintaxis Básica y Tipos de Datos

## Comentarios

```rust
// Comentario de línea única

/*
 Comentario de bloque
 que puede abarcar múltiples líneas
*/
```

## Función main

Todo programa ejecutable en Rust debe tener una función `main` como punto de entrada:

```rust
fn main() {
    // Código aquí
}
```

## Variables y Mutabilidad

Por defecto, las variables son inmutables. Para hacerlas mutables, usa `mut`:

```rust
let x = 5;         // x es inmutable
let mut y = 5;     // y es mutable
y = 6;             // Esto está bien
// x = 6;           // Esto causaría un error de compilación
```

## Tipos de Datos Primitivos

Rust es un lenguaje de tipado estático, pero el compilador suele poder inferir tipos.

### Enteros
| Tipo | Tamaño | Rango |
|------|--------|-------|
| i8 | 8 bits | -128 a 127 |
| i16 | 16 bits | -32,768 a 32,767 |
| i32 | 32 bits | -2,147,483,648 a 2,147,483,647 |
| i64 | 64 bits | -9,223,372,036,854,775,808 a 9,223,372,036,854,775,807 |
| i128 | 128 bits | -170,141,183,460,469,231,731,687,303,715,884,105,728 a 170,141,183,460,469,231,731,687,303,715,884,105,727 |
| isize | Tamaño del puntero | Depende de la arquitectura |
| u8 | 8 bits | 0 a 255 |
| u16 | 16 bits | 0 a 65,535 |
| u32 | 32 bits | 0 a 4,294,967,295 |
| u64 | 64 bits | 0 a 18,446,744,073,709,551,615 |
| u128 | 128 bits | 0 a 340,282,366,920,938,463,463,374,607,431,768,211,455 |
| usize | Tamaño del puntero | Depende de la arquitectura |

### Flotantes
| Tipo | Tamaño | Precisión |
|------|--------|-----------|
| f32 | 32 bits | Simple precisión |
| f64 | 64 bits | Doble precisión (predeterminado) |

### Booleanos
```rust
let verdadero = true;
let falso = false;
```

### Caracteres
```rust
let c = 'z';
let z = 'ℤ';
let corazón_oxidado = '❤';
```

### Tipo Unitario
```rust
let () = (); // El tipo unitario representa un valor vacío
```

## Literales Numéricos

Rust permite varios formatos para facilitar la lectura:

```rust
let decimal = 98_222;
let hex = 0xff;
let octal = 0o77;
let binario = 0b1111_0000;
let byte = b'A'; // Solo para u8
```

### Sufijos de tipo
Puedes especificar explícitamente el tipo con un sufijo:

```rust
let entero: u32 = 42;
let flotante: f64 = 3.14;
```

## Operaciones Aritméticas

```rust
let suma = 5 + 10;
let resta = 95.5 - 4.3;
let multiplicacion = 4 * 30;
let division = 56.7 / 32.2;
let resto = 43 % 5;
```

### Operaciones de asignación compuesta
```rust
let mut a = 5;
a += 3; // a = a + 3
a -= 2;
a *= 4;
a /= 2;
a %= 3;
```

## Sombras (Shadowing)

Puedes declarar una nueva variable con el mismo nombre, ocultando la anterior:

```rust
let x = 5;
let x = x + 1; // Ahora x es 6
let x = x * 2; // Ahora x es 12

// El tipo también puede cambiar en cada sombra
let espacios = "   ";
let espacios = espacios.len(); // Ahora espacios es un entero (3)
```

## Constantes

Las constantes son similares a variables inmutables pero se declaran con `const` y deben tener su tipo anotado. Son válidas durante toda la ejecución del programa en cualquier alcance.

```rust
const TRES_HORAS_EN_SEGUNDOS: u32 = 60 * 60 * 3;

fn main() {
    println!("Hay {} segundos en tres horas", TRES_HORAS_EN_SEGUNDOS);
}
```

## Sintaxis de Funciones

Ya vimos la sintaxis básica, pero aquí más detalles:

```rust
fn nombre_funcion(parametro1: Tipo1, parametro2: Tipo2) -> TipoDeRetorno {
    // cuerpo de la función
    // El último expresión es el valor de retorno implícito
    // o puedes usar return explícitamente
}
```

### Funciones sin retorno (tipo unitario)
```rust
fn imprime_mensaje(mensaje: &str) {
    println!("{}", mensaje);
    // Retorna implícitamente ()
}
```

## Control de Flujo

### Expresiones if
```rust
let numero = 3;

if numero < 5 {
    println!("condición verdadera");
} else {
    println!("condición falsa");
}

// if como expresión (devuelve un valor)
let condicion = true;
let numero = if condicion { 5 } else { 6 }; // numero será 5
```

### Bucles
#### Loop infinito
```rust
loop {
    println!("¡Otra vez!");
    // break para salir
}
```

#### While
```rust
let mut numero = 3;
while numero != 0 {
    println!("{}!", numero);
    numero -= 1;
}
println!("LIFTOFF!!!");
```

#### For
```rust
// Rango inclusivo-excluyente (hasta pero sin incluir el límite superior)
for elemento in 0..5 {
    println!("{}", elemento); // Imprime 0,1,2,3,4
}

// Rango inclusivo-inclusivo (con ..=)
for elemento in 0..=5 {
    println!("{}", elemento); // Imprime 0,1,2,3,4,5
}

// Iterando sobre un array
let a = [10, 20, 30, 40, 50];
for elemento in a.iter() {
    println!("el valor es: {}", elemento);
}
```

## Manejo de Entrada y Salida

### Impresión
```rust
print!("Esto no agrega salto de línea"); // Sin salto
println!("Esto agrega un salto de línea"); // Con salto
```

### Lectura desde stdin
Requiere la biblioteca estándar:

```rust
use std::io;

fn main() {
    println!("Por favor ingresa tu nombre.");

    let mut nombre = String::new();

    io::stdin()
        .read_line(&mut nombre)
        .expect("Falló al leer la línea");

    println!("¡Hola, {}!", nombre.trim_end());
}
```

## Ejercicios

1. Escribe un programa que convierta temperaturas entre Celsius y Fahrenheit.
2. Crea una calculadora simple que pueda realizar las cuatro operaciones básicas.
3. Implementa el juego de "Adivina el número" donde la computadora piensa un número y el usuario debe adivinarlo.
4. Escribe un programa que imprima la sucesión de Fibonacci hasta un número dado.