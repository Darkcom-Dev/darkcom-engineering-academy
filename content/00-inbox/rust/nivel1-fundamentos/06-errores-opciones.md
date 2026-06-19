# Manejo de Errores y Opciones

En Rust, el manejo de errores es una parte fundamental del lenguaje y se basa en el principio de que los errores deben ser manejados explícitamente, en lugar de ignorarlos o dejarlos caer en tiempo de ejecución como ocurre con excepciones en otros lenguajes.

## El problema con las excepciones

Muchos lenguajes usan excepciones para manejar errores, pero esto tiene varios problemas:
- Ocultan el flujo normal del código
- Es difícil saber qué funciones pueden lanzar qué excepciones
- Pueden hacer que el código sea menos eficiente
- Es fácil olvidarse de manejarlas

## El enfoque de Rust: Tipos de retorno explícitos

En lugar de excepciones, Rust usa tipos de retorno especiales para indicar que una operación puede fallar:
- `Option<T>` para cuando una operación puede no tener un valor
- `Result<T, E>` para cuando una operación puede tener éxito o fallar con información de error

## `Option<T>`: Cuando algo puede estar ausente

`Option<T>` representa un valor que puede ser o presente (`Some(T)`) o ausente (`None`).

### Definición

```rust
enum Option<T> {
    Some(T),
    None,
}
```

### Uso común
```rust
fn primera_letra(s: &str) -> Option<char> {
    s.chars().next() // Retorna None si la cadena está vacía, Some(c) si tiene caracteres
}

// Uso
fn main() {
    let letra = primera_letra("hola");
    match letra {
        Some(c) => println!("Primera letra: {}", c),
        None => println!("La cadena está vacía"),
    }
}
```

### Métodos útiles de Option
```rust
fn main() {
    let x: Option<i32> = Some(5);
    let y: Option<i32> = None;

    // is_some y is_none
    println!("x.is_some() = {}", x.is_some()); // true
    println!("y.is_none() = {}", y.is_none()); // true

    // unwrap (devuelve el valor o causa panic)
    println!("x.unwrap() = {}", x.unwrap()); // 5
    // println!("y.unwrap() = {}", y.unwrap()); // ¡PANIC!

    // expect (como unwrap pero con mensaje personalizado)
    println!("x.expect(\"x debería tener un valor\") = {}", x.expect("x debería tener un valor"));
    // println!("y.expect(\"y debería tener un valor\") = {}", y.expect("y debería tener un valor")); // ¡PANIC!

    // unwrap_or (devuelve el valor o un valor por defecto)
    println!("x.unwrap_or(0) = {}", x.unwrap_or(0)); // 5
    println!("y.unwrap_or(0) = {}", y.unwrap_or(0)); // 0

    // unwrap_or_else (devuelve el valor o calcula un valor por defecto)
    println!("x.unwrap_or_else(|| 10) = {}", x.unwrap_or_else(|| 10)); // 5
    println!("y.unwrap_or_else(|| 10) = {}", y.unwrap_or_else(|| 10)); // 10

    // map (aplica una función si es Some)
    let x2 = x.map(|val| val * 2);
    println!("x2 = {:?}", x2); // Some(10)
    
    let y2 = y.map(|val| val * 2);
    println!("y2 = {:?}", y2); // None

    // and_then (encadena operaciones que retornan Option)
    fn duplica_si_par(x: i32) -> Option<i32> {
        if x % 2 == 0 {
            Some(x * 2)
        } else {
            None
        }
    }
    
    let resultado = x.and_then(duplica_si_par); // None porque 5 es impar
    println!("resultado = {:?}", resultado);
    
    let z = Some(4);
    let resultado2 = z.and_then(duplica_si_par); // Some(8) porque 4 es par
    println!("resultado2 = {:?}", resultado2);

    // filter (retorna None si no cumple el predicado)
    let filtrado = x.filter(|&x| x > 3); // Some(5) porque 5 > 3
    println!("filtrado = {:?}", filtrado);
    
    let filtrado2 = x.filter(|&x| x > 10); // None porque 5 !> 10
    println!("filtrado2 = {:?}", filtrado2);
}
```

## `Result<T, E>`: Cuando algo puede tener éxito o fallar

`Result<T, E>` representa una operación que puede tener éxito (`Ok(T)`) o fallar (`Err(E)`).

### Definición
```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

### Uso común con archivos

```rust
use std::fs::File;
use std::io;

fn leer_archivo(ruta: &str) -> Result<String, io::Error> {
    let mut archivo = File::open(ruta)?; // El operador ? propaga errores
    let mut contenido = String::new();
    archivo.read_to_string(&mut contenido)?; // El operador ? propaga errores
    Ok(contenido)
}

// Equivalente sin el operador ?
fn leer_archivo_sin_preguntar(ruta: &str) -> Result<String, io::Error> {
    let mut archivo = match File::open(ruta) {
        Ok(archivo) => archivo,
        Err(e) => return Err(e),
    };
    
    let mut contenido = String::new();
    match archivo.read_to_string(&mut contenido) {
        Ok(_) => Ok(contenido),
        Err(e) => Err(e),
    }
}
```

### Métodos útiles de Result

```rust
use std::num::ParseIntError;

fn main() {
    let resultado: Result<i32, ParseIntError> = "42".parse();
    
    // is_ok y is_err
    println!("resultado.is_ok() = {}", resultado.is_ok()); // true
    println!("resultado.is_err() = {}", resultado.is_err()); // false
    
    // unwrap (devuelve el valor o causa panic)
    println!("resultado.unwrap() = {}", resultado.unwrap()); // 42
    // let error: Result<i32, ParseIntError> = "no es un número".parse();
    // println!("error.unwrap() = {}", error.unwrap()); // ¡PANIC!
    
    // expect (como unwrap pero con mensaje personalizado)
    println!("resultado.expect(\"debería ser un número\") = {}", resultado.expect("debería ser un número"));
    
    // unwrap_or (devuelve el valor o un valor por defecto)
    let resultado_default = "no es un número".parse::<i32>().unwrap_or(0);
    println!("resultado_default = {}", resultado_default); // 0
    
    // unwrap_or_else (devuelve el valor o calcula un valor por defecto)
    let resultado_default2 = "no es un número".parse::<i32>().unwrap_or_else(|e| {
        println!("Error al parsear: {}", e);
        0
    });
    println!("resultado_default2 = {}", resultado_default2);
    
    // map (transforma el valor de Ok)
    let doblado = resultado.map(|x| x * 2);
    println!("doblado = {:?}", doblado); // Ok(84)
    
    // map_err (transforma el error de Err)
    let error_mensaje = resultado.map_err(|e| format!("Error de parseo: {}", e));
    // Como resultado es Ok, el error no se transforma
    println!("error_mensaje = {:?}", error_mensaje);
    
    // and_then (encadena operaciones que retornan Result)
    fn multiplicar_por_dos(x: i32) -> Result<i32, ParseIntError> {
        Ok(x * 2)
    }
    
    let encadenado = resultado.and_then(multiplicar_por_dos);
    println!("encadenado = {:?}", encadenado); // Ok(84)
    
    // or_else (provee una alternativa si es Err)
    let alternativo = "no es un número".parse::<i32>().or_else(|_| Ok(42));
    println!("alternativo = {:?}", alternativo); // Ok(42)
}

// El operador ? para propagación de errores
fn ejemplo_propagacion() -> Result<i32, io::Error> {
    let mut archivo = File::open("config.txt")?;
    let mut contenido = String::new();
    archivo.read_to_string(&mut contenido)?;
    let numero: i32 = contenido.trim().parse()?;
    Ok(numero * 2)
}

// Equivalente sin el operador ?
fn ejemplo_propagacion_sin_preguntar() -> Result<i32, io::Error> {
    let mut archivo = match File::open("config.txt") {
        Ok(archivo) => archivo,
        Err(e) => return Err(e),
    };
    
    let mut contenido = String::new();
    match archivo.read_to_string(&mut contenido) {
        Ok(_) => {},
        Err(e) => return Err(e),
    }
    
    let numero = match contenido.trim().parse() {
        Ok(numero) => numero,
        Err(e) => return Err(std::io::Error::new(std::io::ErrorKind::InvalidData, e)),
    };
    
    Ok(numero * 2)
}
```

## `Panic!`: Cuando algo sale terriblemente mal

Aunque el manejo explícito de errores es preferible, hay situaciones en las que continuar no tiene sentido y es mejor terminar el programa inmediatamente. Para esto, Rust proporciona la macro `panic!`.

### Uso de panic!

```rust
fn main() {
    // Esto causará un panic y terminará el programa
    panic!("¡Algo salió terriblemente mal!");
    
    // Código después del panic nunca se ejecuta
    println!("Esto nunca se imprimirá");
}
```

### Panic con índice fuera de límites

```rust
fn main() {
    let v = vec![1, 2, 3];
    
    // Esto causará un panic: index out of bounds: the len is 3 but the index is 99
    let _ = v[99];
}
```

### Personalizando el comportamiento de panic
Por defecto, cuando ocurre un panic, el programa se desacelera (se ejecuta el proceso de unwinding) y luego termina. Podemos cambiar esto para que termine inmediatamente:

En Cargo.toml:
```toml
[profile.release]
panic = 'abort'  // En lugar del default 'unwind'
```

O establecer la variable de entorno:
```bash
RUST_BACKTRACE=1 cargo run  # Para mostrar el backtrace cuando ocurre un panic
```

## Estrategias de manejo de errores

### 1. Propagar errores con el operador ?
Ideal cuando llamas a funciones que pueden fallar y quieres que el error se propague hacia arriba.

```rust
fn procesar_configuracion() -> Result<(), Box<dyn std::error::Error>> {
    let mut archivo = File::open("config.json")?;
    let mut contenido = String::new();
    archivo.read_to_string(&mut contenido)?;
    let config: Config = serde_json::from_str(&contenido)?;
    
    // Procesar la configuración...
    
    Ok(())
}
```

### 2. Manejar errores específicos
Cuando necesitas responder de manera específica a ciertos tipos de errores.

```rust
fn abrir_archivo_o_crear_predeterminado() -> Result<File, io::Error> {
    match File::open("datos.txt") {
        Ok(archivo) => Ok(archivo),
        Err(error) => match error.kind() {
            io::ErrorKind::NotFound => File::create("datos.txt"),
            otros_errores => Err(otros_errores),
        }
    }
}
```

### 3. Usar unwrap y expect con cuidado
Solo en situaciones donde sepas que un error es imposible o cuando estés prototipando.

```rust
fn main() {
    // Solo si estás absolutamente seguro de que el archivo existe y es legible
    let contenido = fs::read_to_string("archivo_seguro.txt")
        .expect("Archivo seguro debería existir y ser legible");
    
    // Procesar contenido...
}
```

### 4. Crear tus propios tipos de error
Para aplicaciones más grandes, es útil definir tus propios tipos de error.

```rust
use thiserror::Error;

#[derive(Error, Debug)]
enum MiError {
    #[error("Archivo no encontrado: {0}")]
    ArchivoNoEncontrado(String),
    
    #[error("Error de parseo: {0}")]
    ErrorDeParseo(#[from] std::num::ParseIntError),
    
    #[error("Error de E/S: {0}")]
    ErrorDeIo(#[from] std::io::Error),
    
    #[error("Valor fuera de rango: {0}")]
    ValorFueraDeRango(i32),
}

fn procesar_numero(entrada: &str) -> Result<i32, MiError> {
    let archivo = File::open(entrada)?;
    // ... procesar archivo ...
    let numero: i32 = entrada.parse()?;
    
    if numero < 0 || numero > 100 {
        return Err(MiError::ValorFueraDeRango(numero));
    }
    
    Ok(numero)
}
```

## Buenas prácticas

1. **Siempre maneja los errores explícitamente**: No uses `unwrap()` en código de producción excepto en casos muy específicos.
2. **Usa el operador ? para propagar errores**: Hace el código más limpio y legible.
3. **Proporciona contexto en los errores**: Cuando uses `map_err` o crees tus propios errores, agrega información útil para depuración.
4. **No ignores errores silenciosamente**: Si decides ignorar un error, hazlo explícitamente y documenta por qué.
5. **Usa Result para errores recuperables y panic para errores irrecuperables**: Si el programa puede continuar razonablemente, usa Result. Si continuar no tiene sentido, usa panic.
6. **Aprovecha los combinadores**: map, and_then, or_else, etc. hacen que el manejo de errores sea más expresivo.

## Ejercicios

1. Implementa una función que lea un archivo de configuración en formato JSON y devuelva una estructura de configuración, manejando apropiadamente todos los posibles errores (archivo no encontrado, JSON mal formado, etc.).

2. Crea una función que analice una fecha en formato "DD/MM/AAAA" y devuelva un Result con la fecha válida o un error específico si el formato es incorrecto o la fecha no es válida (como 31/02/2023).

3. Implementa una calculadora que maneje expresiones aritméticas simples (+, -, *, /) y devuelva un Result que indique tanto el resultado como posibles errores (división por cero, sintaxis inválida, etc.).

4. Crea un tipo de error personalizado para una aplicación de gestión de inventario que incluya variantes como: ProductoNoEncontrado, CantidadInsuficiente, CódigoDeProductoInvalido, etc.

5. Implementa una función que conecte a una base de datos SQLite, ejecute una consulta y devuelva los resultados, manejando apropiadamente los errores de conexión, consulta y mapeo.

6. Usa el patrón de combinación de Option y Result para implementar una función que busque un usuario por ID en un mapa, verifique que tenga permisos suficientes, y luego devuelva su información de contacto.