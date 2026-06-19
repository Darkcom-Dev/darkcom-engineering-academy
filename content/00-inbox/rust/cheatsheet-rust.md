# Rust Cheatsheet

Referencia rápida del lenguaje Rust y su ecosistema.

---

## Índice

- [Comandos](#comandos)
- [Hola Mundo](#hola-mundo)
- [Variables y Mutabilidad](#variables-y-mutabilidad)
- [Constantes](#constantes)
- [Shadowing](#shadowing)
- [Tipos de Datos](#tipos-de-datos)
  - [Enteros](#enteros)
  - [Flotantes](#flotantes)
  - [Booleanos](#booleanos)
  - [Caracteres](#caracteres)
- [Tipos Compuestos](#tipos-compuestos)
  - [Tuplas](#tuplas)
  - [Arrays](#arrays)
  - [Vectores](#vectores)
  - [Strings y &str](#strings-y-str)
- [Funciones](#funciones)
- [Structs](#structs)
- [Tuple Structs](#tuple-structs)
- [Implementación (`impl`)](#implementacin-impl)
- [Enums](#enums)
- [Macros](#macros)
- [Macros de Impresión](#macros-de-impresin)
- [Colores ANSI](#colores-ansi)
- [Control de Flujo](#control-de-flujo)
- [Expresiones vs Sentencias](#expresiones-vs-sentencias)
- [Módulos y Pruebas](#mdulos-y-pruebas)
- [Crates y Cargo](#crates-y-cargo)
- [IDEs y Extensiones](#ides-y-extensiones)

---

## Comandos

### rustc (compilador directo)

```bash
rustc --version              # Versión instalada
rustc archivo.rs             # Compila a binario
./archivo.rs                 # Ejecuta el binario (Linux/Mac)
```

### Cargo (gestor de proyecto)

```bash
cargo new nombre_proyecto          # Nuevo proyecto binario
cargo new nombre_proyecto --lib    # Nueva librería
cargo run                          # Compila y ejecuta
cargo check                        # Verifica que compila (más rápido que build)
cargo build                        # Compila en modo debug
cargo build --release              # Compila con optimizaciones
```

> El ejecutable debug queda en `target/debug/` y el release en `target/release/`.

---

## Hola Mundo

```rust
fn main() {
    println!("Hola mundo!");
}
```

---

## Variables y Mutabilidad

Por defecto las variables son **inmutables**.

```rust
let x = 5;           // Inmutable
let mut y = 10;      // Mutable
y += 1;

let z: u8;           // Declaración sin asignar
z = 42;              // Asignación posterior (válida una sola vez)
```

### Tipado

Rust es de **tipado estático**. El tipo se puede inferir o declarar explícitamente:

```rust
let a = 5;           // Infiere i32
let b: u8 = 5;       // Explícito: unsigned 8-bit
```

---

## Constantes

```rust
const MAX_PUNTOS: u32 = 100_000;
```

- Siempre requieren tipo explícito.
- Pueden ser globales (fuera de `fn main`).
- Se nombran en `MAYÚSCULAS_CON_GUIONES`.

---

## Shadowing

Reutilizar el mismo nombre de variable, cambiando tipo o mutabilidad:

```rust
let x = 10;
let x = x + 5;       // Sombra a la anterior
let x = "ahora texto";
```

---

## Tipos de Datos

### Enteros

| Tipo   | Con signo | Sin signo |
|--------|-----------|-----------|
| 8-bit  | `i8`      | `u8`      |
| 16-bit | `i16`     | `u16`     |
| 32-bit | `i32`     | `u32`     |
| 64-bit | `i64`     | `u64`     |
| 128-bit| `i128`    | `u128`    |
| arquitectura | `isize` | `usize` |

Literales enteros:

```rust
let decimal = 98_222;     // Separador visual _
let hex     = 0xff;       // 0x
let octal   = 0o77;       // 0o
let binary  = 0b1111_0000; // 0b
let byte    = b'A';       // u8 (solo ASCII)
```

### Flotantes

| Tipo | Precisión |
|------|-----------|
| `f32`| 32 bits   |
| `f64`| 64 bits (default) |

```rust
let x = 2.0;    // f64
let y: f32 = 3.0;
```

### Booleanos

```rust
let verdadero = true;
let falso: bool = false;
```

### Caracteres

```rust
let c = 'z';       // comilla simple
let emoji = '😎';   // Unicode de 4 bytes
```

---

## Tipos Compuestos

### Tuplas

Longitud fija, tipos heterogéneos.

```rust
let tupla = (500, "hola", 3.14);
let (a, b, c) = tupla;            // Destructuración
println!("{}", tupla.1);          // Acceso por índice: "hola"
```

### Arrays

Longitud fija, tipo homogéneo.

```rust
let arr = [1, 2, 3];
let primero = arr[0];
```

### Vectores (`Vec`)

Longitud dinámica, tipo homogéneo.

```rust
let mut v = Vec::new();
v.push(10);
v.push(20);

let v2 = vec![1, 2, 3];
println!("{}", v2[1]);
```

### Strings y &str

```rust
let literal: &str = "hola";               // Cadena estática
letString: String = String::from("hola"); // Cadena dinámica
let texto = "mundo".to_string();          // Conversión
println!("{}", texto.len());              // Largo en bytes
```

---

## Funciones

```rust
fn suma(a: i32, b: i32) -> i32 {
    a + b     // Expresión (sin punto y coma) → valor de retorno
}

// Con return explícito:
fn suma_explicita(a: i32, b: i32) -> i32 {
    return a + b;
}
```

- Los parámetros **siempre** llevan tipo.
- El tipo de retorno se indica con `->`.
- La última expresión (sin `;`) es el valor de retorno.

---

## Structs

```rust
struct Persona {
    nombre: String,
    email: String,
    edad: u8,
    activo: bool,
}

let p = Persona {
    nombre: String::from("Ana"),
    email: String::from("ana@mail.com"),
    edad: 30,
    activo: true,
};

println!("{}", p.nombre);
```

### Tuple Structs

```rust
struct Punto(i32, i32, i32);

let origen = Punto(0, 0, 0);
println!("{}", origen.0);  // Acceso por índice
```

### Implementación (`impl`)

```rust
impl Persona {
    fn año_nacimiento(&self, año_actual: u16) -> u16 {
        año_actual - self.edad as u16
    }
}

println!("{}", p.año_nacimiento(2025));
```

---

## Enums

```rust
enum Color {
    Rojo,
    Azul,
    Verde,
}

enum RedSocial {
    Facebook(String),
    Instagram(String),
    Twitter(String),
}

let color = Color::Azul;
let red   = RedSocial::Facebook(String::from("fb.com"));
```

---

## Macros

Se identifican por el signo `!`.

```rust
macro_rules! saludar {
    () => {
        println!("¡Hola!");
    };
    ($nombre:expr) => {
        println!("¡Hola, {}!", $nombre);
    };
}

saludar!();
saludar!("Carlos");
```

---

## Macros de Impresión

| Macro       | Comportamiento                         |
|-------------|----------------------------------------|
| `print!()`  | Imprime sin salto de línea             |
| `println!()`| Imprime con salto de línea (`\n`)      |
| `eprintln!()`| Imprime en `stderr`                    |
| `todo!()`   | Marca código pendiente (panic en runtime) |
| `unreachable!()` | Código que no debería ejecutarse     |

```rust
print!("Hola ");           // "Hola "
println!("mundo");         // "mundo\n"
todo!("Implementar esto"); // Provoca panic
```

---

## Colores ANSI

Usar secuencias de escape `\x1b[` + código + `m`:

```rust
println!("\x1b[31mRojo\x1b[0m");
println!("\x1b[32mVerde\x1b[0m");
println!("\x1b[34mAzul\x1b[0m");
println!("\x1b[33mAmarillo\x1b[0m");
println!("\x1b[1;5;33;41mParpadeo + fondo rojo\x1b[0m");
```

| Efecto               | Código     |
|----------------------|------------|
| Reset                | `\x1b[0m` |
| Negrita              | `1`        |
| Parpadeo             | `5`        |
| Color texto (ej. rojo) | `31`    |
| Color fondo (ej. rojo) | `41`    |

---

## Control de Flujo

### if / else if / else

```rust
let n = 5;
if n > 10 {
    println!("grande");
} else if n > 0 {
    println!("positivo");
} else {
    println!("cero o negativo");
}

// if como expresión:
let resultado = if n > 0 { "positivo" } else { "no positivo" };
```

### while

```rust
let mut i = 0;
while i < 5 {
    println!("{}", i);
    i += 1;
}
```

### for

```rust
for i in 0..5 {
    println!("{}", i);
}

let arr = [10, 20, 30];
for elem in arr {
    println!("{}", elem);
}
```

---

## Expresiones vs Sentencias

- **Sentencia** — instrucción que no devuelve valor (termina en `;`).
- **Expresión** — devuelve un valor y **no** lleva `;` al final.

```rust
let y = {
    let x = 3;
    x + 1           // Expresión: devuelve 4
};
```

---

## Módulos y Pruebas

```rust
// lib.rs
pub fn suma(left: usize, right: usize) -> usize {
    left + right
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_suma() {
        assert_eq!(suma(2, 2), 4);
    }
}
```

```bash
cargo test    # Ejecuta todos los tests
```

---

## Crates y Cargo

[Crates.io](https://crates.io) es el registro oficial de paquetes Rust.

```bash
cargo add nombre_crate                          # Añade dependencia
cargo add serde --features derive               # Añade con feature específica
```

Ejemplo de `Cargo.toml`:

```toml
[dependencies]
serde = { version = "1.0", features = ["derive"] }
rand = "0.8"
```

---

## IDEs y Extensiones

Recomendados: **IntelliJ** (Rust plugin) o **VS Code**.

Extensiones para VS Code:

- `rust-analyzer` — autocompletado, diagnóstico, navegación
- `Even Better TOML` — resalta `Cargo.toml`
- `crates` — info de versiones de dependencias
- `CodeLLDB` — depurador (Rust, C, C++)
- `C/C++` — soporte adicional

---

> _Cheatsheet generado a partir de apuntes de aprendizaje de Rust._

