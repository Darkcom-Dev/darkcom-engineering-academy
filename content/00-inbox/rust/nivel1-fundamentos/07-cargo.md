# Gestión de Paquetes con Cargo

Cargo es el sistema de gestión de paquetes y construcción de Rust. Es una herramienta esencial que maneja muchas tareas incluyendo la compilación de código, descarga de dependencias y generación de documentación.

## ¿Qué es Cargo?

Cargo es tres cosas en una:
1. **Sistema de construcción** (build system): Compila tu código
2. **Gestor de dependencias**: Descarga y compila las bibliotecas que tu código necesita
3. **Gestor de paquetes**: Publica y distribuye tus propios paquetes en crates.io

## Creando un nuevo proyecto

```bash
# Crear un nuevo proyecto ejecutable
cargo new nombre_del_proyecto
cd nombre_del_proyecto

# Crear un nuevo proyecto de biblioteca
cargo new nombre_de_la_biblioteca --lib
```

Esto crea la siguiente estructura:
```
nombre_del_proyecto/
├─ Cargo.toml
├─ src/
│  └─ main.rs   # o lib.rs para bibliotecas
└─ .git/        # y archivo .gitignore si git está disponible
```

## El manifiesto: Cargo.toml

El archivo `Cargo.toml` es el manifiesto de tu proyecto. Está escrito en formato TOML y contiene toda la metadata que Cargo necesita.

### Secciones básicas

```toml
[package]
name = "mi_proyecto"           # Nombre del paquete (único en crates.io)
version = "0.1.0"             # Versión semántica
edition = "2021"              # Edición de Rust (2015, 2018, 2021)
authors = ["Tu Nombre <tu@email.com>"]
description = "Una breve descripción de tu proyecto"
documentation = "https://docs.example.com/mi_proyecto"
readme = "README.md"
license = "MIT OR Apache-2.0"
repository = "https://github.com/tuusuario/mi_proyecto"
keywords = ["rust", "ejemplo", "tutorial"]
categories = ["command-line-utilities", "development-tools"]

[dependencies]
# Lista de dependencias externas
serde = { version = "1.0", features = ["derive"] }
tokio = { version = "1.0", features = ["full"] }

[dev-dependencies]
# Dependencias solo para testing, ejemplos y benchmarks
pretty_assertions = "1.0"
predicates = "3.0"

[features]
# Características personalizadas (features)
default = ["std"]
std = []

[profile.dev]
# Configuración para el perfil de desarrollo (cargo build/cargo run)
opt-level = 0
debug = true
incremental = true

[profile.release]
# Configuración para el perfil de liberación (cargo build --release)
opt-level = 3
debug = false
incremental = false
lto = true
strip = true
```

### Especificando versiones de dependencias

Cargo usa versiones semánticas y tiene varias formas de especificar dependencias:

```toml
[dependencies]
# Versión exacta
paquete = "=1.2.3"

# Versión mínima (cualquier versión >= 1.2.3)
paquete = "^1.2.3"   # Equivale a >=1.2.3, <2.0.0 (recomendado)
paquete = "~1.2.3"   # Equivale a >=1.2.3, <1.3.0

# Rango de versiones
paquete = ">=1.2.0, <1.5.0"
paquete = "1.2.*"    # Cualquier versión 1.2.x

# Desde un repositorio Git
paquete = { git = "https://github.com/rust-lang/serde.git" }
paquete = { git = "https://github.com/rust-lang/serde.git", branch = "next" }
paquete = { git = "https://github.com/rust-lang/serde.git", tag = "v1.0.0" }
paquete = { git = "https://github.com/rust-lang/serde.git", rev = "a1b2c3d" }

# Desde una ruta local
paquete = { path = "vendor/mi_dependencia" }
```

## Comandos esenciales de Cargo

### Construcción y ejecución
```bash
# Compilar en modo desarrollo (rápido, sin optimizaciones)
cargo build

# Compilar y ejecutar
cargo run

# Compilar en modo liberación (optimizado)
cargo build --release

# Ejecutar sin recompilar (si no han cambiado los fuentes)
cargo run --quiet
```

### Gestión de dependencias
```bash
# Añadir una dependencia
cargo add serde --features derive

# Añadir una dependencia de desarrollo
cargo add pretty_assertions --dev

# Actualizar dependencias a las últimas versiones compatibles
cargo update

# Actualizar una dependencia específica
cargo update -p serde

# Mostrar el árbol de dependencias
cargo tree

# Verificar dependencias sin compilar
cargo check
```

### Testing
```bash
# Ejecutar todas las pruebas
cargo test

# Ejecutar solo pruebas cuyo nombre coincida con un filtro
cargo test palabra_clave

# Ejecutar pruebas en modo liberación
cargo test --release

# Mostrar salida de pruebas exitosas (normalmente se oculta)
cargo test -- --show-output

# Ejecutar solo tests de unidad
cargo test --lib

# Ejecutar solo tests de integración
cargo test --tests
```

### Documentación
```bash
# Generar documentación
cargo doc

# Generar documentación y abrirla en el navegador
cargo doc --open

# Generar documentación incluyendo dependencias
cargo doc --open --document-private-items
```

### Benchmarks
```bash
# Ejecutar benchmarks (requiere feature de nightly)
cargo bench

# Ejecutar solo ciertos benchmarks
cargo bench nombre_del_benchmark
```

### Publicando en crates.io
```bash
# Iniciar sesión en crates.io
cargo login

# Empaquetar el crate (ver qué se incluirá)
cargo package --list

# Publicar en crates.io
cargo publish

# Publicar por primera vez (necesita confirmación)
cargo publish --allow-dirty

# Retirar una versión (yanking)
cargo yank --versione 1.2.3
cargo yank --versione 1.2.3 --undo  # Deshacer el yanking
```

## Perfiles de compilación

Cargo tiene cuatro perfiles predefinidos que se pueden personalizar en Cargo.toml:

1. **dev**: Para `cargo build` y `cargo run` (desarrollo)
2. **release**: Para `cargo build --release` (liberación)
3. **test**: Para `cargo test` (pruebas)
4. **bench**: Para `cargo bench` (benchmarks)

### Personalización de perfiles
```toml
[profile.dev]
opt-level = 1          # Nivel de optimización (0-3)
debug = true           # Incluir información de depuración
incremental = true     # Compilación incremental

[profile.release]
opt-level = 3
debug = false
incremental = false
lto = true             # Link Time Optimization
strip = true           # Eliminar símbolos de depuración
panic = 'unwind'       # o 'abort'

# Perfiles personalizados (se pueden crear perfiles adicionales)
[profile.bench]
opt-level = 3
debug = false
```

## Trabajo en equipo y CI/CD

### Archivo Cargo.lock
Este archivo se genera automáticamente y contiene las versiones exactas de todas las dependencias. Debe ser incluido en el control de versiones para asegurar builds reproducibles.

### Integración continua
Ejemplo básico para GitHub Actions (`.github/workflows/ci.yml`):
```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: dtolnay/rust-toolchain@stable
    - name: Build
      run: cargo build --verbose
    - name: Test
      run: cargo test --verbose
    - name: Clippy
      run: cargo clippy -- -D warnings
    - name: Format
      run: cargo fmt -- --check
```

## Buenas prácticas

1. **Versiona exactamente tus aplicaciones**: En aplicaciones finales, considera usar versiones exactas de dependencias para builds reproducibles.
2. **Usa rangos flexibles en bibliotecas**: En bibliotecas, usa `^` o `~` para permitir actualizaciones seguras.
3. **Ejecuta `cargo check` frecuentemente**: Es más rápido que `cargo build` ya que solo verifica sin generar código.
4. **Mantén tus dependencias actualizadas**: Usa `cargo outdated` (de la crate `cargo-outdated`) para verificar actualizaciones.
5. **Documenta tu API pública**: Usa `cargo doc --open` para revisar cómo se verá tu documentación.
6. **Sigue las convenciones de nombres**: Usa snake_case para nombres de crates y funciones.
7. **Aprovecha las características (features)**: Para habilitar funcionalidades opcionales sin añadir dependencias innecesarias.
8. **Publica versiones estables**: Usa versionado semántico correctamente y considera hacer un período de pre-lanzamiento con versiones `-rc`.

## Recursos adicionales
- https://doc.rust-lang.org/cargo/
- https://crates.io/
- https://github.com/rust-lang/cargo
- https://doc.rust-lang.org/cargo/reference/registries.html