# Redirecciones y Manejo de Errores en Bash

Entender cómo redirigir la entrada y salida, así como manejar errores adecuadamente, es esencial para crear scripts robustos y profesionales. Este documento cubre las diversas técnicas de redirección disponibles en bash y las mejores prácticas para el manejo de errores.

## Descriptores de Archivo en Bash

Bash utiliza descriptores de archivo para manejar flujos de entrada/salida. Los más importantes son:

| Descriptor | Nombre | Flujo Standard | Uso Típico |
|------------|--------|----------------|------------|
| 0 | stdin | Entrada Estándar | Lectura de entrada |
| 1 | stdout | Salida Estándar | Salida normal |
| 2 | stderr | Salida de Error Estándar | Mensajes de error |
| 3-9 | - | Descriptores disponibles | Para uso personalizado |

## Redirección Básica

### Redirección de Salida (stdout)
```bash
# Sobrescribir un archivo con la salida
comando > archivo.txt

# Anexar la salida a un archivo existente
comando >> archivo.txt

# Redirigir y crear el archivo si no existe (misma behavior que >)
comando 1> archivo.txt
# o equivalentemente
comando > archivo.txt
```

### Redirección de Error (stderr)
```bash
# Sobrescribir un archivo con los errores
comando 2> error.txt

# Anexar los errores a un archivo existente
comando 2>> error.txt
```

### Redirección de Ambos Flujos
```bash
# Redirigir stdout y stderr a archivos separados
comando > salida.txt 2> errores.txt

# Redirigir ambos flujos al mismo archivo
comando > todo.txt 2>&1
# o equivalentemente (más conciso)
comando &> todo.txt
# o también
comando > todo.txt 2> todo.txt  # Menos eficiente pero funcional
```

### Redirección de Entrada (stdin)
```bash
# Leer entrada desde un archivo en lugar del teclado
comando < archivo.txt

# Usando here document como entrada
comando << DELIM
línea de entrada 1
línea de entrada 2
DELIM
```

## Descriptores de Archivo Personalizados

Además de los descriptores estándar (0, 1, 2), podemos crear nuestros propios descriptores para operaciones avanzadas:

### Abrir Descriptores Personalizados
```bash
# Abrir descriptor 3 para lectura
exec 3< archivo_entrada.txt

# Abrir descriptor 4 para escritura
exec 4> archivo_salida.txt

# Abrir descriptor 5 para lectura y escritura
exec 5<> archivo_actualizar.txt
```

### Usar Descriptores Personalizados
```bash
# Leer desde descriptor 3
read -u 3 linea
echo "Leído: $linea"

# Escribir a descriptor 4
echo "Esta línea va al descriptor 4" >&4

# Leer y escribir en descriptor 5
read -u 5 linea
echo "Procesando: $linea" >&5
```

### Cerrar Descriptores Personalizados
```bash
# Cerrar descriptor 3
exec 3<&-

# Cerrar descriptor 4
exec 4>&-

# Cerrar descriptor 5
exec 5<&-
```

## Here Documents y Here Strings

### Here Document (<<)
Permite pasar múltiples líneas de entrada a un comando:

```bash
# Sintaxis básica
comando << DELIM
línea 1
línea 2
línea 3
DELIM

# Sin expansión de variables (usando comillas en el delimitador)
comando << 'DELIM'
Texto con $VARIABLES que no se expanden
DELIM

# Con sangrado (eliminando tabs iniciales)
comando <<- DELIM
    Línea con tabulación inicial
    Otra línea con tabulación
    La tabulación inicial se eliminará
DELIM
```

### Here String (<<<)
Pasar una sola cadena como entrada:

```bash
# Similar a: echo "cadena" | comando
comando <<< "Esta es la cadena de entrada"

# Con variables
NOMBRE="Juan"
comando <<< "Hola $NOMBRE, ¿cómo estás?"
```

## Manejo de Errores en Bash

### Verificación de Código de Salida
Cada comando devuelve un código de salida donde 0 indica éxito y cualquier otro valor indica error:

```bash
# Verificación básica
comando
if [ $? -eq 0 ]; then
    echo "El comando tuvo éxito"
else
    echo "El comando falló con código: $?"
fi

# Forma más concisa
if comando; then
    echo "Éxito"
else
    echo "Falló"
fi

# Negación de condición
if ! comando; then
    echo "El comando falló como se esperaba"
else
    echo "El comando tuvo éxito inesperadamente"
fi
```

### Uso de `set -e` para Salida Automática al Error
```bash
#!/bin/bash
# Salir inmediatamente si cualquier comando falla
set -e

comando1  # Si falla, el script se detiene aquí
comando2  # Nunca se ejecuta si comando1 falla
comando3  # Nunca se ejecuta si comando1 o comando2 fallan
```

### Excepciones a `set -e`
`set -e` no causa salida en ciertos contextos:
```bash
# Estas situaciones NO causan salida incluso con set -e:
if comando; then     # La condición de un if
while comando; do    # La condición de un while
until comando; do    # La condición de un until
comando || verdadero # El lado izquierdo de un ||
comando && verdadero # El lado izquierdo de un &&
```

### Manejo Selectivo de Errores con `set -e`
Para evitar que `set -e` cause salida en comandos específicos donde esperamos fallos:

```bash
set -e

# Estos comandos no causarán salida incluso si fallan:
comando || true
comando || exit 1  # Aún podemos controlar el flujo
if comando; then
    :
fi

# O usando la sintaxis de comando || alternativa
comando || {
    echo "Advertencia: comando falló pero continuamos"
    # Código de manejo de error específico
}

# También podemos desactivar temporalmente set -e
set +e
comando_que_puede_fallar
set -e
```

### Uso de `set -u` para Detectar Variables No Definidas
```bash
#!/bin/bash
# Salir si se intenta usar una variable no definida
set -u

echo $VARIABLE_DEFINIDA  # Funciona si está definida
echo $VARIABLE_NO_DEFINIDA  # Causa salida inmediatamente con set -e
```

### Uso de `set -o pipefail` para Tuberías
Por defecto, el código de salida de una tubería es el del último comando. Con `pipefail`, es el último comando que falló:

```bash
#!/bin/bash
# Sin pipefail: el código de salida será 0 (éxito de cat)
# false | true | false  # Código de salida: 0 (falso)

# Con pipefail: el código de salida será 1 (primer falso)
set -o pipefail
false | true | false  # Código de salida: 1

# Esto es especialmente útil en scripts donde queremos detectar
# fallos en cualquier parte de una tubería
```

### Funciones de Manejo de Errores Reutilizables
```bash
#!/bin/bash
# Configuración de manejo de errores robusto
set -euo pipefail  # Combina las tres opciones más útiles

# Función para registrar mensajes
log() {
    local nivel="$1"
    shift
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] [$nivel] $*"
}

# Función para manejar errores fatales
die() {
    log "ERROR" "$*"
    exit 1
}

# Función para advertencias
warn() {
    log "ADVERTENCIA" "$*"
}

# Función para información
info() {
    log "INFO" "$*"
}

# Uso en el script
main() {
    info "Iniciando proceso"
    
    # Operación que podría fallar
    if ! comando_riesgoso; then
        warn "comando_riesgoso falló, intentando alternativa"
        if ! comando_alternativo; then
            die "Ambos comandos fallaron"
        fi
    fi
    
    info "Proceso completado exitosamente"
}

main "$@"
```

## Ejemplos Prácticos de Redirección y Manejo de Errores

### Script de Registro con Rotación Simplificada
```bash
#!/bin/bash
set -euo pipefail

LOG_DIR="/var/log/miapp"
LOG_FILE="$LOG_DIR/aplicacion.log"
MAX_SIZE=$((10 * 1024 * 1024))  # 10 MB

# Asegurar que el directorio de logs existe
mkdir -p "$LOG_DIR"

# Rotar el log si es demasiado grande
if [ -f "$LOG_FILE" ] && [ $(stat -c%s "$LOG_FILE") -ge $MAX_SIZE ]; then
    mv "$LOG_FILE" "$LOG_FILE.$(date +%Y%m%d_%H%M%S)"
    # Opcional: comprimir el log rotado
    # gzip "$LOG_FILE.$(date +%Y%m%d_%H%M%S)"
fi

# Ejecutar la aplicación y registrar tanto stdout como stderr
{
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] INICIO DE SESIÓN"
    # Aquí iría el comando principal de la aplicación
    echo "Aplicación ejecutándose..."
    sleep 2
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] FIN DE SESIÓN"
} >> "$LOG_FILE" 2>&1

# Mantener solo los últimos 7 días de logs (opcional)
find "$LOG_DIR" -name "aplicacion.log.*" -mtime +7 -delete 2>/dev/null || true
```

### Script de Backup con Manejo Robusto de Errores
```bash
#!/bin/bash
set -euo pipefail

# Configuración
ORIGEN="/home/usuario/documentos importantes"
DESTINO="/backups"
RETENCION=7  # Días a mantener backups
FECHA=$(date +"%Y%m%d_%H%M%S")
NOMBRE_BACKUP="documentos_$FECHA.tar.gz"

# Funciones de ayuda
log_info() { echo "[INFO] $(date +'%H:%M:%S') $*"; }
log_error() { echo "[ERROR] $(date +'%H:%M:%S') $*" >&2; }
die() { log_error "$*"; exit 1; }

# Verificar requisitos
log_info "Iniciando backup de $ORIGEN"

if [ ! -d "$ORIGEN" ]; then
    die "El directorio de origen no existe: $ORIGEN"
fi

if [ ! -d "$DESTINO" ]; then
    log_info "Creando directorio de destino: $DESTINO"
    mkdir -p "$DESTINO" || die "No se pudo crear el directorio de destino"
fi

# Crear el backup
log_info "Creando archivo de backup: $NOMBRE_BACKUP"
if ! tar -czf "$DESTINO/$NOMBRE_BACKUP" -C "$(dirname "$ORIGEN")" "$(basename "$ORIGEN")"; then
    die "Error al crear el archivo de backup"
fi

log_info "Backup creado exitosamente: $DESTINO/$NOMBRE_BACKUP"

# Eliminar backups antiguos
log_info "Eliminando backups antiguos (más de $RETENCION días)"
if ! find "$DESTINO" -name "documentos_*.tar.gz" -mtime +$RETENCION -delete; then
    log_warning "Algunos backups antiguos no pudieron ser eliminados"
fi

log_info "Proceso de backup completado"
```

## Buenas Prácticas

1. **Siempre usa `set -euo pipefail`** al inicio de tus scripts para un manejo de errores robusto:
   ```bash
   #!/bin/bash
   set -euo pipefail  # Mejor combinación para scripts de producción
   ```

2. **Separa stdout y stderr** en tus scripts para facilitar el procesamiento y el registro:
   ```bash
   # Salida normal (información para el usuario)
   echo "Procesando archivo: $archivo" >&1
   
   # Mensajes de diagnóstico y progreso
   echo "Procesando registro $i de $total" >&2
   
   # Mensajes de error reales
   echo "Error: No se puede leer $archivo" >&2
   ```

3. **Usa descriptores de archivo personalizados** para operaciones avanzadas de E/S cuando necesites manejar múltiples flujos simultáneamente.

4. **Aprovecha los here documents** para crear archivos de configuración o contenido múltiple de manera limpia y legible.

5. **Maneja explícitamente los códigos de salida** de comandos críticos en lugar de depender únicamente de `set -e` cuando necesites lógica de recuperación específica.

6. **Registra operaciones importantes** tanto en stdout como en archivos de log para facilitar la auditoría y el diagnóstico posterior.

7. **Prueba el manejo de errores** intencionalmente provocando fallos para verificar que tu script se comporte correctamente ante situaciones adversas.

## Ejercicios

1. Crea un script que procese un archivo CSV y genere un reporte, implementando un manejo robusto de errores que registre tanto éxito como fallos en un archivo de log separado.

2. Desarrolla un script de sincronización de directorios que use `rsync` con opciones apropiadas, registre todas las operaciones y notifique por correo electrónico si ocurren errores durante la sincronización.

3. Implementa un script que monitoree el uso de disco en puntos de montaje críticos y genere alertas cuando el uso supere ciertos umbrales, registrando todo en un archivo de log con rotación automática.

4. Crea un script que descargue archivos desde múltiples URLs, verifique su integridad con checksums, y continúe con las siguientes descargas incluso si alguna falla, registrando detalladamente qué tuvo éxito y qué falló.
## Relacionados:
- [[standard-io]] #related 
- [[standard-output]] #related 
- [[stdout-single]] #related 