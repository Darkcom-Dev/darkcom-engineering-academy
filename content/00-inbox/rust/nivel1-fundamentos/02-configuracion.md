# Configuración del Entorno de Desarrollo en Rust

## Instalación de Rust

La forma recomendada de instalar Rust es mediante **rustup**, el instalador y gestor de versiones oficial.

### En Linux y macOS
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### En Windows
Descargar e instalar [rustup-init.exe](https://sh.rustup.rs) desde https://www.rust-lang.org/tools/install

## Componentes instalados

Después de instalar rustup tendrás:

- **rustc**: El compilador de Rust
- **cargo**: El gestor de paquetes y sistema de construcción
- **rustup**: El gestor de versiones de Rust
- **rust-documentation**: Documentación local (accessible con `rustup doc`)

## Actualización de Rust

Para actualizar a la última versión estable:
```bash
rustup update
```

Para instalar versiones específicas (beta, nightly):
```bash
rustup install beta
rustup install nightly
rustup default beta  # o nightly
```

## Configuración de IDE

### Visual Studio Code
1. Instalar la extensión **"Rust Analyzer"**
2. Opcional: "Even Better TOML" para editar Cargo.toml
3. Opcional: "Crates" para gestionar dependencias

### IntelliJ IDEA / CLion
1. Instalar el plugin **"Rust"**
2. Configurar el SDK de Rust en las preferencias

## Primeros comandos útiles

```bash
# Ver versión de Rust y Cargo
rustc --version
cargo --version

# Crear un nuevo proyecto
cargo new hola_mundo
cd hola_mundo

# Compilar y ejecutar
cargo run

# Solo compilar (sin ejecutar)
cargo build

# Compilar con optimizaciones para release
cargo build --release

# Ejecutar pruebas
cargo test

# Generar documentación
cargo doc --open

# Formatear código según el estilo oficial
cargo fmt

# Revisar código con clippy (linter)
cargo clippy
```

## Creación de un proyecto de biblioteca
```bash
cargo new mi_libreria --lib
```

## Estructura de un proyecto Cargo típico
```
mi_proyecto/
├─ Cargo.toml       # Manifestode dependencias y configuración
├─ src/
│  ├─ main.rs       # Punto de entrada para ejecutables
│  └─ lib.rs        # Para bibliotecas (cuando se usa --lib)
└─ target/          # Directorio de compilación (generado automáticamente)
```

## El archivo Cargo.toml
```toml
[package]
name = "mi_proyecto"
version = "0.1.0"
edition = "2021"  # Edición de Rust a usar

[dependencies]
# Lista de dependencias (crates.io)
serde = { version = "1.0", features = ["derive"] }
tokio = { version = "1.0", features = ["full"] }

[dev-dependencies]
# Dependencias solo para testing y ejemplos
pretty_assertions = "1.0"
```

## Recursos adicionales
- https://www.rust-lang.org/tools/install
- https://doc.rust-lang.org/cargo/
- https://github.com/rust-lang/rust-analyzer