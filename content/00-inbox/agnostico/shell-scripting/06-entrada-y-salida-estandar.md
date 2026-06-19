# Entrada y Salida Estándar en Bash

Entender cómo funciona la entrada y salida estándar es fundamental para crear scripts de shell efectivos y flexibles. Este concepto permite que los scripts interactúen con el usuario, reciban datos de otros comandos mediante tuberías, y redirijan su salida a archivos o otros procesos.

## Los Tres Flujos Estándar

Cada proceso en Linux tiene tres flujos de entrada/salida predefinidos:

| Flujo | Descriptor | Nombre | Uso Típico |
|-------|------------|--------|------------|
| **Entrada Estándar (stdin)** | 0 | stdin | Lectura de entrada del usuario o de otros comandos |
| **Salida Estándar (stdout)** | 1 | stdout | Salida normal del programa |
| **Salida de Error Estándar (stderr)** | 2 | stderr | Mensajes de error y diagnóstico |

## Manejo de Argumentos de Línea de Comandos

Los scripts pueden recibir parámetros cuando se ejecutan, los cuales se acceden mediante variables especiales:

### Variables de Posición
| Variable | Descripción |
|----------|-------------|
| `$0` | Nombre del script mismo |
| `$1`, `$2`, ..., `$n` | Primer, segundo, ..., n-ésimo argumento |
| `$@` | Todos los argumentos como palabras separadas |
| `$*` | Todos los argumentos como una sola palabra |
| `$#` | Número total de argumentos |

```bash
#!/bin/bash
# ejemplo_args.sh

echo "Nombre del script: $0"
echo "Primer argumento: $1"
echo "Segundo argumento: $2"
echo "Todos los argumentos: $@"
echo "Número de argumentos: $#"

# Accediendo a argumentos específicos mediante array
args=("$@")  # Convierte argumentos en un array
echo "Tercer argumento: ${args[2]}"  # Índice 0-based
echo "Cuarto argumento: ${args[3]}"
```

### Procesamiento de Opciones con Getopts
Para manejar opciones complejas (como `-h`, `-f archivo`, etc.), se usa `getopts`:

```bash
#!/bin/bash
# ejemplo_getopts.sh

# Variable para almacenar resultados
INPUT_FILE=""
OUTPUT_FILE=""
VERBOSE=false

# Definir opciones esperadas
# : después de la letra indica que requiere argumento
while getopts ":i:o:v" opt; do
  case $opt in
    i)
      INPUT_FILE="$OPTARG"
      echo "Archivo de entrada: $INPUT_FILE" >&2
      ;;
    o)
      OUTPUT_FILE="$OPTARG"
      echo "Archivo de salida: $OUTPUT_FILE" >&2
      ;;
    v)
      VERBOSE=true
      echo "Modo verbose activado" >&2
      ;;
    \?)
      echo "Opción inválida: -$OPTARG" >&2
      exit 1
      ;;
    :)
      echo "La opción -$OPTARG requiere un argumento." >&2
      exit 1
      ;;
  esac
done

# Opcional: eliminar las opciones procesadas de $@
shift $((OPTIND-1))

# Resto del script...
if [ -n "$INPUT_FILE" ]; then
    echo "Procesando archivo: $INPUT_FILE"
fi
```

## Lectura desde Entrada Estándar (Stdin)

Hay varias formas de leer desde stdin en bash:

### Usando `read`
El comando `read` lee una línea de entrada y la asigna a variables:

```bash
# Lectura básica
echo -n "Ingresa tu nombre: "
read NOMBRE
echo "Hola, $NOMBRE"

# Lectura con timeout
if read -t 5 -p "Respuesta rápida (5 segundos): " RESPUESTA; then
    echo "Tu respuesta: $RESPUESTA"
else
    echo -e "\nTiempo agotado"
fi

# Lectura segura (para contraseñas o datos sensibles)
read -s -p "Contraseña: " CONTRASENA
echo  # Nueva línea después de la entrada silenciosa
echo "Contraseña recibida"

# Lectura de múltiples variables en una línea
echo -n "Ingresa tres números separados por espacios: "
read A B C
echo "Suma: $((A + B + C))"

# Lectura que preserva espacios iniciales y finales
# Nota: Por defecto, read elimina espacios iniciales y finales
# Para preservarlos, establecer IFS a cadena vacía temporalmente
while IFS= read -r line; do
    echo "Línea leída: '$line'"
done
```

### Lectura de Archivos
Cuando un script necesita procesar un archivo, puede leerlo directamente o usar redirección:

```bash
# Método 1: Redirección de entrada
while IFS= read -r linea; do
    echo "Procesando: $linea"
done < archivo.txt

# Método 2: Usando cat y tubería (menos eficiente)
cat archivo.txt | while IFS= read -r linea; do
    echo "Procesando: $linea"
done

# Método 3: Asignando a una variable (solo para archivos pequeños)
CONTENIDO=$(< archivo.txt)
echo "$CONTENIDO"

# Método 4: Using mapfile (bash 4+)
mapfile -t LINEAS < archivo.txt
for linea in "${LINEAS[@]}"; do
    echo "Procesando: $linea"
done
```

## Escritura a Salida Estándar (Stdout y Stderr)

### Salida Normal (stdout)
Por defecto, los comandos como `echo` y `printf` envían su salida a stdout:

```bash
# Salida básica
echo "Este es un mensaje normal"

# Salida con formato usando printf
printf "Nombre: %-10s Edad: %02d\n" "Juan" 25
printf "Precio: %'.2f euros\n" 19.99

# Redirigiendo stdout a un archivo
echo "Este mensaje va a un archivo" > salida.txt
echo "Este mensaje se anexa" >> salida.txt
```

### Salida de Error (stderr)
Los mensajes de error deben enviarse a stderr para que puedan ser separados de la salida normal:

```bash
# Enviando mensaje a stderr
echo "Este es un mensaje de error" >&2
echo "Otra forma: " "Mensaje de error" >&2

# Función reutilizable para mensajes de error
error() {
    echo "ERROR: $*" >&2
}

# Uso
if [ ! -f "archivo_requerido.txt" ]; then
    error "No se encontró el archivo requerido"
    exit 1
fi
```

### Redirección de Flujos
```bash
# Redirigir stdout a un archivo y stderr a otro
./script.py > salida.log 2> errores.log

# Redirigir ambos stdout y stderr al mismo archivo
./script.py > todo.log 2>&1
# o equivalentemente
./script.py &> todo.log

# Redirigir stderr a /dev/null (suprimir errores)
./script.py 2>/dev/null

# Redirigir stdout a /dev/null (suprimir salida normal)
./script.py >/dev/null
```

## Here Documents (Heredoc)
Los here documents permiten pasar múltiples líneas de entrada a un comando:

```bash
# Sintaxis básica
comando << DELIM
línea 1
línea 2
línea 3
DELIM

# Con expansión de variables y comandos
NOMBRE="Juan"
cat << EOF
Hola $NOMBRE,
Hoy es $(date +%A).
EOF

# Sin expansión (usando comillas simples en el delimitador)
cat << 'EOF'
Esto no se expande: $NOMBRE $(date)
EOF

# Con sangrado (usando <<- y tabs)
cat <<- EOF
    Esta línea tendrá el tabulación inicial removida
    Así como esta
EOF
```

## Ejemplos Prácticos

### Filtro que Procesa Archivos mengumumkan Stdin
```bash
#!/bin/bash
# numero_lineas.sh - Añade números de línea a la entrada

# Si se proporciona un archivo como argumento, léelo; de lo contrario, usa stdin
while IFS= read -r linea || [ -n "$linea" ]; do
    # Contador de líneas (mantiene estado entre iteraciones)
    ((contador++))
    printf "%4d: %s\n" "$contador" "$linea"
done < "${1:-/dev/stdin}"
```

### Script que Trabaja con Tuberías
```bash
#!/bin/bash
# contador_palabras.sh - Cuenta palabras en la entrada

# Inicializar contador
total=0

# Leer entrada línea por línea
while IFS= read -r linea || [ -n "$linea" ]; do
    # Contar palabras en la línea (dividiendo por espacios)
    # La expansión de $linea sin comillas divide en palabras
    set -- $linea
    total=$((total + $#))
done < "${1:-/dev/stdin}"

echo "Total de palabras: $total"
```

### Interactive Prompt with Validation
```bash
#!/bin/bash
# solicitando_entrada.sh - Pide y valida entrada del usuario

# Función para obtener un entero positivo
obtener_entero_positivo() {
    local prompt="$1"
    local valor
    
    while true; do
        read -p "$prompt" valor
        # Verificar que sea un entero positivo
        if [[ $valor =~ ^[0-9]+$ ]] && [ "$valor" -gt 0 ]; then
            echo "$valor"
            return 0
        else
            echo "Error: Por favor ingrese un número entero positivo" >&2
        fi
    done
}

# Uso de la función
EDAD=$(obtener_entero_positivo "Ingrese su edad: ")
ALTURA=$(obtener_entero_positivo "Ingrese su altura en cm: ")

echo "Gracias. Usted tiene $EDAD años y mide $ALTURA cm."
```

## Buenas Prácticas

1. **Siempre verifica si stdin está disponible** cuando esperas entrada interactiva:
   ```bash
   if [ -t 0 ]; then
       # stdin está conectado a una terminal - entrada interactiva segura
       read -p "Continuar? [s/N]: " respuesta
   else
       # stdin está redirigido o proveniente de una tubería
       # Usar valor predeterminado o salir con error
       respuesta="N"
   fi
   ```

2. **Separa stdout y stderr** en tus scripts para facilitar el procesamiento:
   ```bash
   # En lugar de mezclar todo en stdout
   echo "Procesando archivo $archivo"
   
   # Hazlo así
   echo "Procesando archivo $archivo"  # Información normal -> stdout
   if [ ! -r "$archivo" ]; then
       echo "Error: No se puede leer $archivo" >&2  # Error -> stderr
       exit 1
   fi
   ```

3. **Usa `printf` en lugar de `echo`** para salida formateada cuando necesites control preciso:
   ```bash
   # printf es más predecible y portable que echo
   printf "%s\n" "Mensaje con % y \ caracteres"
   ```

4. **Considera el uso de `mapfile` o `readarray`** (bash 4+) para leer archivos completos en arrays cuando sea apropiado:
   ```bash
   # Lee todo el archivo en un array (un elemento por línea)
   mapfile -t lineas < archivo.txt
   # Ahora puedes acceder a lineas[0], lineas[1], etc.
   ```

5. **Maneja adecuadamente el caso de entrada vacía** al usar `read`:
   ```bash
   # El bucle while read falla en la última línea si no termina en newline
   # La solución es usar || [ -n "$linea" ]
   while IFS= read -r linea || [ -n "$linea" ]; do
       # Procesar $linea
   done < archivo
   ```

## Ejercicios

1. Crea un script que funcione tanto como filtro (leyendo de stdin) como procesador de archivos (leyendo de archivos especificados como argumentos).
2. Desarrolla un script que convierta temperaturas entre Celsius y Fahrenheit, aceptando la unidad como argumento y el valor como entrada estándar o argumento.
3. Implementa un script que haga un seguimiento simple de tareas, permitiendo agregar, listar y marcar tareas como completadas, almacenando todo en un archivo de datos.
4. Crea un script que analice un log de acceso web y genere un reporte de las direcciones IP más activas, aceptando el archivo de log como argumento o leyendo de stdin si no se proporciona.
## Ejemplos Completos

### Script de apoyo: `stdout_single.sh`

Script auxiliar que muestra comentarios, heredocs y ejecuta comandos con y sin error:

```bash
#!/bin/bash

# Comentario de línea única

: '
Comentario de múltiples líneas
'

cat << DELIM
Script de apoyo para ejemplos de redirección
DELIM

ls -l          # Comando que funciona (stdout)
lsl -l         # Comando que falla (stderr)
```

### Script: `standard-output.sh`

Ejemplo de redirección de stdout y stderr a archivos separados:

```bash
#!/bin/bash

cat << DELIM
Este script muestra el uso de varias salidas.
Por favor verifique los archivos:
  lista.txt   (stdout)
  error.txt   (stderr)
  output.txt  (stdout + stderr combinados)
DELIM

# Redirigir stdout a archivo
ls -l 1>lista.txt

# Redirigir stderr a archivo
lsl -l 2>error.txt

# Redirigir ambos al mismo archivo
bash stdout_single.sh >output.txt 2>&1
```

### Script: `standard-io.sh`

Ejemplo de manejo de argumentos y lectura desde stdin:

```bash
#!/bin/bash

cat << DELIM
Este script usa varios ejemplos de entrada de información.
Ingrese como parámetro el nombre de un archivo .txt.
Luego se le pedirá su nombre y apellido para saludarlo.
DELIM

# Variables de argumentos
echo "Nombre del script: $0"
args=("$@")
echo "Primer argumento: ${args[0]}"
echo "Segundo argumento: ${args[1]}"
echo "Todos los argumentos: $@"
echo "Número de argumentos: $#"

# Leer archivo línea por línea
while IFS= read -r line; do
    echo "$line"
done < "${1:-/dev/stdin}"

# Leer entrada del usuario
echo "Ingrese su primer nombre"
read fname
echo "Ingrese su primer apellido"
read lname
echo "Hola $fname $lname"
```
