# Estructuras de Datos Compuestas

Rust proporciona varias formas de agrupar múltiples valores en un solo tipo. Las más importantes son tuplas, arrays, slices, vectores, structs y enums.

## Tuplas

Las tuplas son colecciones de valores de tipos potencialmente diferentes, con un tamaño fijo conocido en tiempo de compilación.

### Sintaxis básica
```rust
let tupla: (i32, f64, u8) = (500, 6.4, 1);
// O bien, dejando que el compilador infiera los tipos:
let tupla = (500, 6.4, 1);
```

### Acceso a elementos
```rust
let tup = (500, 6.4, 1);

// Acceso por desestructuración
let (x, y, z) = tup;
println!("El valor de y es: {}", y);

// Acceso por índice (usando punto)
let quinientos = tup.0;
let seis_punto_cuatro = tup.1;
let uno = tup.2;
```

### Tuplas vacías y unitarias
```rust
let unidad = (); // Tipo unitario, útil para funciones que no retornan nada significativo
```

### Cuándo usar tuplas
- Devolver múltiples valores desde una función
- Agrupar temporalmente unos pocos valores relacionados
- Cuando necesitas un tipo de dato simple y de tamaño fijo

## Arrays

Los arrays son colecciones de elementos del mismo tipo con un tamaño fijo conocido en tiempo de compilación.

### Sintaxis básica

```rust
let a = [1, 2, 3, 4, 5]; // [i32; 5]
let b: [i32; 3] = [1, 2, 3]; // Especificando tipo explícitamente
let c = [3; 5]; // Crea [3, 3, 3, 3, 3] - cinco elementos con valor 3
```

### Acceso a elementos

```rust
let a = [1, 2, 3, 4, 5];

let primero = a[0]; // 1
let segundo = a[1]; // 2

// Acceso fuera de límites causa panic en tiempo de ejecución
// let fuera = a[10]; // ¡ERROR EN TIEMPO DE EJECUCIÓN!

// Acceso seguro con get() que retorna Option
match a.get(5) {
    Some(&valor) => println!("Elemento: {}", valor),
    None => println!("No hay elemento en ese índice"),
}
```

### Iterando sobre arrays

```rust
let a = [10, 20, 30, 40, 50];

// Por valor (mueve los elementos fuera del array - solo para tipos Copy)
for elemento in a {
    println!("valor: {}", elemento);
}

// Por referencia (no toma ownership)
for elemento in &a {
    println!("valor: {}", elemento);
}

// Por referencia mutable
let mut a = [10, 20, 30, 40, 50];
for elemento in &mut a {
    *elemento += 1;
}
println!("{:?}", a); // [11, 21, 31, 41, 51]
```

### Limitaciones de los arrays
- Tamaño fijo conocido en tiempo de compilación
- Todas las operaciones de inserción/eliminación requieren crear un nuevo array
- Útiles cuando conoces exactamente cuántos elementos necesitas

## Slices

Los slices son vistas a una secuencia contigua de elementos, sin tomar ownership. Son más flexibles que los arrays porque su tamaño no necesita ser conocido en tiempo de compilación.

### Slices de arrays

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..4]; // Índices 1, 2, 3 (hasta pero no incluyendo 4)
println!("{:?}", slice); // [2, 3, 4]

let slice_completo = &a[..]; // Equivalente a &a[0..a.len()]
let desde_inicio = &a[..3];  // Equivalente a &a[0..3]
let hasta_fin = &a[2..];     // Equivalente a &a[2..a.len()]

// Tipo del slice: &[i32]
```

### Slices de strings (string slices o &str)

```rust
let s = String::from("hola mundo");

let hello = &s[0..5]; // "hola"
let world = &s[6..11]; // "mundo"

// Los slices de string son &str
let saludo: &str = "hola"; // Literal de string es un &str

// Nota: Los índices en string slices deben caer en límites de caracteres UTF-8 válidos
let saludo = "hola ¿qué tal?";
let h = &saludo[0..1]; // "h" - OK
// let invalido = &saludo[0..2]; // ¡ERROR! "h" ocupa 1 byte, "o" ocupa 1 byte, pero " " ocupa 1 byte más
// Los caracteres no ASCII pueden ocupar múltiples bytes
```

### Por qué usar slices
- No toman ownership de los datos
- Permiten trabajar con secuencias sin copiar datos
- Tamaño determinado en tiempo de ejecución
- Compatibles con arrays, `Vec<String>`, y otros tipos que implementen deref a [T]

## Vectores (`Vec<T>`)

Los vectores son arrays redimensionables que almacenan sus datos en el heap. Son la colección más utilizada en Rust cuando necesitas una lista redimensionable.

### Creación de vectores

```rust
let mut v: Vec<i32> = Vec::new(); // Vector vacío
v.push(5);
v.push(6);
v.push(7);
println!("{:?}", v); // [5, 6, 7]

// Macro vec! para inicialización conveniente
let v = vec![1, 2, 3, 4, 5];
println!("{:?}", v); // [1, 2, 3, 4, 5]

// Vector con capacidad inicial (evita realocaciones si conoces el tamaño aproximado)
let mut v = Vec::with_capacity(10);
```

### Acceso a elementos

```rust
let v = vec![1, 2, 3, 4, 5];

let tercero: &i32 = &v[2]; // Retorna referencia
println!("El tercer elemento es {}", tercero);

// O usando get() para acceso seguro que retorna Option
match v.get(2) {
    Some(tercero) => println!("El tercer elemento es {}", tercero),
    None => println!("No hay tercer elemento"),
}

// Intento de acceso fuera de límites con [] causa panic
// let no_existe = &v[10]; // ¡PANIC!
```

### Modificando vectores

```rust
let mut v = vec![1, 2, 3, 4, 5];

v.push(6);           // Añade al final
v.pop();             // Elimina y retorna el último elemento
v.insert(2, 10);     // Inserta en posición 2
v.remove(2);         // Elimina elemento en posición 2 y lo retorna
v.clear();           // Elimina todos los elementos
```

### Iterando sobre vectores

```rust
let v = vec![100, 32, 57];

// Por valor (consume el vector - mueve los elementos fuera)
for i in v {
    println!("{}", i);
}
// Ahora v no es usable

// Por referencia (no toma ownership)
let v = vec![100, 32, 57];
for i in &v {
    println!("{}", i);
}
// v sigue siendo usable

// Por referencia mutable
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;
}
println!("{:?}", v); // [150, 82, 107]
```

### Vectores con enums para tipos heterogéneos

Aunque los vectores suelen contener elementos del mismo tipo, podemos usar enums para lograr heterogeneidad:

```rust
enum Dato {
    Texto(String),
    Entero(i32),
    Flotante(f64),
}

let fila = vec![
    Dato::Texto(String::from("Nombre")),
    Dato::Entero(42),
    Dato::Flotante(3.14),
];
```

## Strings

Rust tiene dos tipos principales para manejar texto: `String` (crecible, propiedad) y `&str` (slice de string, vista).

### String vs &str

```rust
let mut s = String::from("hola"); // String crecible
s.push_str(", mundo");           // Añade un slice de string
s.push('!');                     // Añade un caracter
println!("{}", s);               // "hola, mundo!"

// Conversión de &str a String
let saludo = "hola";
let saludo_string = saludo.to_string();
// o
let saludo_string = String::from(saludo);

// Conversión de String a &str (deref coercion)
fn toma_slice(s: &str) {
    println!("Recibido: {}", s);
}

let mi_string = String::from("hola");
toma_slice(&mi_string); // &String se coercea automáticamente a &str
toma_slice(&mi_string[0..2]); // También podemos pasar slices
```

### Operaciones comunes con String

```rust
let mut s = String::from("foo");
s.push_str("bar");    // "foobar"
s.push('!');          // "foobar!"
s += " más texto";    // "foobar! más texto"

// Concatenación con +
let s1 = String::from("hola");
let s2 = String::from("mundo");
let s3 = s1 + &s2;    // Nota: s1 pierde ownership, s2 es prestado
// s1 ya no es usable después de esto

// Mejor para concatenación múltiple: format!
let s1 = String::from("hola");
let s2 = String::from("mundo");
let s3 = String::from("!");
let s = format!("{} {} {}", s1, s2, s3);
// s1, s2, s3 siguen siendo utilizables
```

### Índices y slices en String

```rust
let hola = "hola ¿cómo estás?";

// No podemos hacer hola[0] porque los índices en String son complejos (UTF-8)
// Pero sí podemos tomar slices:
let h = &hola[0..1]; // "h"
let ola = &hola[1..4]; // "ola"

// Para acceder por caracteres Unicode válidos:
for c in hola.chars() {
    println!("{}", c);
}

// Para acceder por bytes:
for b in hola.bytes() {
    println!("{}", b);
}

// Para acceder por posiciones de caracteres (más costoso):
for (i, c) in hola.char_indices() {
    println!("{}: {}", i, c);
}
```

## Structs

Los structs permiten crear tipos de datos personalizados que pueden agrupar múltiples valores relacionados.

### Definiendo y usando structs

```rust
struct Usuario {
    nombre: String,
    email: String,
    activo: bool,
    edad: u8,
}

// Creando una instancia
let mut usuario1 = Usuario {
    nombre: String::from("Alice"),
    email: String::from("alice@ejemplo.com"),
    activo: true,
    edad: 30,
};

// Actualizando campos
usuario1.email = String::from("alice.nueva@ejemplo.com");

// Accediendo a campos
println!("{} tiene {} años", usuario1.nombre, usuario1.edad);
```

### Sintaxis de actualización estructural

```rust
let usuario2 = Usuario {
    email: String::from("otro@ejemplo.com"),
    ..usuario1 // Copia los剩余 campos de usuario1
    // Nota: usuario1 pierde la propiedad de los campos de tipo String
    // Si los campos fueran Copy, usuario1 seguiría siendo usable
};
```

### Métodos en structs

```rust
struct Rectangulo {
    ancho: u32,
    alto: u32,
}

impl Rectangulo {
    // Método asociado (constructor)
    fn nuevo(ancho: u32, alto: u32) -> Rectangulo {
        Rectangulo { ancho, alto }
    }

    // Método que toma &self (solo lectura)
    fn area(&self) -> u32 {
        self.ancho * self.alto
    }

    // Método que toma &mut self (para modificar)
    fn escalar(&mut self, factor: u32) {
        self.ancho *= factor;
        self.alto *= factor;
    }

    // Método asociado que no toma self (función utility)
    fn puede_contener(&self, otro: &Rectangulo) -> bool {
        self.ancho > otro.ancho && self.alto > otro.alto
    }
}

// Uso
fn main() {
    let mut rect = Rectangulo::nuevo(30, 50);
    
    println!("El área es: {}", rect.area()); // Método
    
    rect.escalar(2);
    println!("Después de escalar: {} x {}", rect.ancho, rect.alto);
    
    let otro = Rectangulo::nuevo(10, 20);
    println!("¿Puede contener? {}", rect.puede_contener(&otro)); // Método asociado
}
```

### Structs tupla y structs unitarios

```rust
// Struct tupla
struct Color(i32, i32, i32);
struct Punto(i32, i32);

let negro = Color(0, 0, 0);
let origen = Punto(0, 0);

// Struct unitario (útil para marcas)
struct SiempreIgual;

let instancia = SiempreIgual;
```

## Enums y match

Los enums permiten definir un tipo que puede ser uno de varios variantes.

### Definiendo y usando enums

```rust
enum Mensaje {
    Salir,
    Mover { x: i32, y: i32 },
    Escribir(String),
    CambiarColor(i32, i32, i32),
}

let msg1 = Mensaje::Salir;
let msg2 = Mensaje::Mover { x: 10, y: 20 };
let msg3 = Mensaje::Escribir(String::from("hola"));
let msg4 = Mensaje::CambiarColor(255, 0, 255);
```

### Métodos en enums

```rust
enum Mensaje {
    Salir,
    Mover { x: i32, y: i32 },
    Escribir(String),
    CambiarColor(i32, i32, i32),
}

impl Mensaje {
    fn llamar(&self) {
        match self {
            Mensaje::Salir => println!("Saliendo..."),
            Mensaje::Mover { x, y } => {
                println!("Moviendo a coordenadas x:{}, y:{}", x, y)
            }
            Mensaje::Escribir(texto) => println!("Mensaje: {}", texto),
            Mensaje::CambiarColor(r, g, b) => {
                println!("Cambiando color a RGB({}, {}, {})", r, g, b)
            }
        }
    }
}

// Uso
let m = Mensaje::Escribir(String::from("hello"));
m.llamar(); // Imprime "Mensaje: hello"
```

### Opción (`Option<T>`) - El enum más importante de Rust
```rust
// Definición simplificada de Option
enum Option<T> {
    Some(T),
    None,
}

// Uso práctico
fn dividir(dividendo: f64, divisor: f64) -> Option<f64> {
    if divisor == 0.0 {
        None
    } else {
        Some(dividendo / divisor)
    }
}

// Manejo de Option con match
fn main() {
    match dividir(10.0, 2.0) {
        Some(resultado) => println!("10.0 / 2.0 = {}", resultado),
        None => println!("No se puede dividir por cero"),
    }

    match dividir(10.0, 0.0) {
        Some(resultado) => println!("10.0 / 0.0 = {}", resultado),
        None => println!("No se puede dividir por cero"),
    }
}

// Métodos útiles de Option
fn main() {
    let x: Option<i32> = Some(5);
    let y: Option<i32> = None;

    println!("x es {:?}", x);
    println!("y es {:?}", y);

    // Unwrap (peligroso - causa panic si es None)
    println!("x.unwrap() = {}", x.unwrap());
    // println!("y.unwrap() = {}", y.unwrap()); // ¡PANIC!

    // Expect (similar a unwrap pero con mensaje personalizado)
    println!("x.expect(\"x debería ser Some\") = {}", x.expect("x debería ser Some"));
    // println!("y.expect(\"y debería ser Some\") = {}", y.expect("y debería ser Some")); // ¡PANIC!

    // Unwrap_or (retorna valor por defecto si es None)
    println!("x.unwrap_or(0) = {}", x.unwrap_or(0));
    println!("y.unwrap_or(0) = {}", y.unwrap_or(0));

    // Unwrap_or_else (llama a una función para generar el valor por defecto)
    println!("x.unwrap_or_else(|| 10) = {}", x.unwrap_or_else(|| 10));
    println!("y.unwrap_or_else(|| 10) = {}", y.unwrap_or_else(|| 10));

    // is_some y is_none
    println!("x.is_some() = {}", x.is_some());
    println!("y.is_none() = {}", y.is_none());

    // map (aplica una función si es Some)
    let x2 = x.map(|val| val * 2);
    println!("x2 = {:?}", x2); // Some(10)
    
    let y2 = y.map(|val| val * 2);
    println!("y2 = {:?}", y2); // None
}
```

### `Result<T, E>` - Para manejo de errores recuperables
```rust
// Definición simplificada de Result
enum Result<T, E> {
    Ok(T),
    Err(E),
}

// Uso con std::fs::File
use std::fs::File;

fn main() {
    let archivo = File::open("hola.txt");
    
    let archivo = match archivo {
        Ok(archivo) => archivo,
        Err(error) => {
            println!("Error al abrir el archivo: {:?}", error);
            return;
        }
    };
    
    // Continuar usando el archivo...
}

// Métodos útiles de Result (similares a Option)
fn main() {
    let resultado: Result<i32 المصادر>
    
    // Unwrap y expect
    println!("resultado.unwrap() = {}", resultado.unwrap());
    // resultado.unwrap_err(); // Para obtener el error
    
    // map y map_err
    let resultado_doblado = resultado.map(|x| x * 2);
    
    // and_then y or_else para encadenar operaciones
}

// El operador ? para propagación de errores
fn leer_username_de_archivo() -> Result<String, std::io::Error> {
    let username_file_result = File::open("username.txt");
    
    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e), // Propagar el error
    };
    
    let mut username = String::new();
    
    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}

// Versión con operador ? (mucho más conciso)
fn leer_username_de_archivo() -> Result<String, std::io::Error> {
    let mut username_file = File::open("username.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}

// Incluso más conciso usando métodos encadenados
fn leer_username_de_archivo() -> Result<String, std::io::Error> {
    let mut username = String::new();
    File::open("username.txt")?.read_to_string(&mut username)?;
    Ok(username)
}
```

## Ejercicios

1. Implementa una estructura `Persona` con campos para nombre, edad y dirección. Añade métodos para:
   - Crear una nueva persona
   - Imprimir la información de la persona
   - Verificar si es mayor de edad
   - Cambiar la dirección

2. Crea un enum `Forma` que pueda representar círculos, rectángulos y triángulos. Implementa un método `area()` que calcule el área de cada forma.

3. Implementa una función que use un `HashMap` para contar la frecuencia de cada palabra en una frase dada.

4. Crea un programa que simule una pila (stack) usando un `Vec<T>` y que permita hacer push, pop y ver el elemento en la cima.

5. Implementa una cola (queue) usando un `VecDeque<T>` y que permita hacer enqueue, dequeue y ver el elemento frontal.

6. Usa structs anidados para representar una dirección de correo electrónico con campos para usuario y dominio, y luego usa esa estructura dentro de una estructura de contacto que incluya nombre, dirección de email y teléfono.