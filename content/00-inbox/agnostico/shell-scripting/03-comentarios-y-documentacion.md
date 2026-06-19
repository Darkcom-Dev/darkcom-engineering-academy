# Comentarios y Documentación en Scripts de Shell

Los comentarios son esenciales para hacer que los scripts sean comprensibles, mantenibles y colaborativos. Aunque bash no tiene soporte nativo para documentación formal como algunos lenguajes, existen varias formas efectivas de comentar y documentar código.

## Tipos de Comentarios en Bash

### Comentarios de Línea Simple
El tipo más común de comentario en bash comienza con `#` y continúa hasta el final de la línea.

```bash
# Este es un comentario de línea simple
VARIABLE="valor"  # También se puede comentar al final de una línea de código
```

### Comentarios de Múltiples Líneas
Bash no tiene una sintaxis específica para comentarios de múltiples líneas, pero hay varias técnicas para lograr este efecto:

#### Método 1: Comentarios en Bloque Usando `: ' ... '`
```bash
: '
Este es un comentario de múltiples líneas
que puede abarcar tantas líneas como necesites
útil para explicaciones detalladas o bloques de código comentados
'
```

#### Método 2: Here Documents (Heredoc) para Comentarios
```bash
<< 'COMENTARIO'
Este es otro método para crear comentarios de múltiples líneas
que no serán interpretados por el shell
útil cuando necesitas incluir caracteres especiales o saltos de línea
COMENTARIO
```

#### Método 3: Comentarios con `if false; then ... fi`
```bash
if false; then
    Este bloque no se ejecutará
    pero sirve como comentario de múltiples líneas
    útil para comentar temporalmente bloques de código
fi
```

## Buenas Prácticas para Comentarios

### ¿Cuándo Comentar?
1. **Explica el "por qué", no el "qué"**: El código debería ser auto-explicativo para lo que hace; los comentarios deben explicar por qué se hizo de esa manera.
   
   ```bash
   # Mal: describe lo obvio
   contador=$((contador + 1))  # Incrementar el contador
   
   # Bueno: explica la razón
   contador=$((contador + 1))  # Incrementar para llevar cuenta de intentos fallidos
   ```

2. **Compleja lógica o algoritmos**: Cuando la implementación no es obvia.
   
   ```bash
   # Algoritmo de ordenamiento burbuja optimizado
   # Complejidad O(n²) pero eficiente para arreglos pequeños o casi ordenados
   for ((i=0; i<${#array[@]}-1; i++)); do
       swapped=false
       for ((j=0; j<${#array[@]}-i-1; j++)); do
           if [[ ${array[j]} -gt ${array[$((j+1))]} ]]; then
               # Intercambio de elementos
               temp=${array[j]}
               array[$j]=${array[$((j+1))]}
               array[$((j+1))]=$temp
               swapped=true
           fi
       done
       # Si no hubo intercambios, el array ya está ordenado
       [[ $swapped == false ]] && break
   done
   ```

3. **Decisiones de diseño importantes**: Cuando se elige una approche sobre otra por razones específicas.
   
   ```bash
   # Usando expresión regular en lugar de bucles para mejor rendimiento
   # en validación de formatos complejos como emails o URLs
   if [[ $email =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
       echo "Email válido"
   fi
   ```

4. **Advertencias y limitaciones**: Cuando hay condiciones especiales o casos que no funcionan.
   
   ```bash
   # ADVERTENCIA: Esta función asume que el archivo de entrada está ordenado
   # Si no lo está, los resultados serán incorrectos
   function procesar_ordenado() {
       # ...
   }
   ```

### Formato y Estilo de Comentarios
1. **Mantén los comentarios actualizados**: Un comentario falso es peor que ningún comentario.
2. **Sé conciso pero claro**: Un buen comentario comunica la información necesaria sin ser verboso.
3. **Usa lenguaje profesional y respetuoso**: Evita comentarios sarcásticos o despectivos.
4. **Alinea los comentarios** cuando comentes múltiples líneas relacionadas:
   
   ```bash
   # Configuración de conexión a base de datos
   DB_HOST="localhost"     # Servidor de base de datos
   DB_PORT="5432"          # Puerto predeterminado de PostgreSQL
   DB_NAME="miaplicacion"  # Nombre de la base de datos
   DB_USER="appuser"       # Usuario de la aplicación
   ```

## Documentación de Scripts

Además de comentarios en línea, es buena práctica incluir documentación al inicio de cada script:

```bash
#!/bin/bash
#================================================================
#
#          FILE: backup_script.sh
# 
#         USAGE: ./backup_script.sh [opciones]
# 
#   DESCRIPTION: Script para realizar copias de seguridad de directorios importantes
#                con compresión, rotación y notificación por email.
# 
#       OPTIONS: ---
#  REQUIREMENTS: --- 
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: Tu Nombre (), 
#       COMPANY: 
#       VERSION: 1.0
#       CREATED: DD/MM/AAAA HH:MM:SS
#      REVISION:  ---
#================================================================

set -euo pipefail  # Mejores prácticas de seguridad y depuración

# ... resto del script ...
```

### Plantilla de Documentación para Scripts
```bash
#!/bin/bash
#================================================================
#
#          FILE: 
# 
#         USAGE: 
# 
#   DESCRIPTION: 
# 
#       OPTIONS: 
#  REQUIREMENTS: 
#          BUGS: 
#         NOTES: 
#        AUTHOR: 
#       COMPANY: 
#       VERSION: 
#       CREATED: 
#      REVISION:  ---
#================================================================
```

## Herramientas para Generar Documentación

### shredoc
Una herramienta que extrae comentarios especiales para generar documentación:

```bash
#!/bin/bash
# <shredoc>
# @name backup_script
# @description Realiza copias de seguridad con compresión y rotación
# @author Tu Nombre
# @version 1.0
# @param   -d  Directorio a respaldar (requerido)
# @param   -o  Directorio de salida (opcional, por defecto ./backups)
# @param   -c  Número de copias a mantener (opcional, por defecto 7)
# @example ./backup_script.sh -d /home/user/documentos -o /mnt/backup -c 14
# </shredoc>
```

### Plantillas de Inicio Rápido
Crear plantillas para nuevos scripts que incluyan la estructura de documentación:

```bash
#!/bin/bash
#================================================================
#
#          FILE: 
# 
#         USAGE: 
# 
#   DESCRIPTION: 
# 
#       OPTIONS: 
#  REQUIREMENTS: 
#          BUGS: 
#         NOTES: 
#        AUTHOR: 
#       COMPANY: 
#       VERSION: 
#       CREATED: 
#      REVISION:  ---
#================================================================

# Salir en caso de error, variable no definida o fallo en pipe
set -euo pipefail

# Variables de configuración
CONFIG_FILE="${HOME}/.config/miapp/config.conf"

# Funciones
usage() {
    cat << EOF
Uso: $0 [opciones]

Este script hace algo útil.

Opciones:
  -h, --help      Mostrar esta ayuda y salir
  -v, --version   Mostrar información de versión
  -d, --debug     Activar modo de depuración

Ejemplos:
  $0 -h
  $0 --version
EOF
}

# Procesamiento de argumentos
while [[ $# -gt 0 ]]; do
    case $1 in
        -h|--help)
            usage
            exit 0
            ;;
        -v|--version)
            echo "miapp version 1.0"
            exit 0
            ;;
        -d|--debug)
            set -x  # Activar modo de depuración
            shift
            ;;
        *)    # Opción desconocida
            echo "Error: Opción desconocida: $1"
            usage
            exit 1
            ;;
    esac
done

# Lógica principal del script
main() {
    # Aquí va la lógica principal
    echo "Ejecutando miapp..."
}

# Ejecutar solo si el script se ejecuta directamente (no se importa)
if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then
    main "$@"
fi
```

## Buenas Prácticas de Documentación

1. **Documenta la interfaz pública**: Para scripts que serán usados por otros, documenta claramente cómo usarlos.
2. **Incluye ejemplos de uso**: Los ejemplos concretos son spesso más útiles que las descripciones abstractas.
3. **Mantén la documentación cerca del código**: La documentación que está lejos del código tiende a quedar desactualizada.
4. **Considera generar documentación automáticamente**: Para proyectos grandes, herramientas como shredoc o incluso Doxygen pueden ayudar.
5. **Documenta valores de retorno y efectos secundarios**: Qué ocurre cuando el script falla o tiene éxito.
6. **Incluye información de autor y licencia**: Important para código que será compartido o reutilizado.

## Ejercicios

1. Toma un script existente y añádele documentación completa siguiendo la plantilla proporcionada.
2. Mejora los comentarios de un script complejo explicando el "por qué" detrás de decisiones importantes.
3. Crea una plantilla personalizada para tus scripts que incluya tu información de contacto y licencia preferida.
4. Experimenta con las diferentes técnicas para comentarios de múltiples líneas y elige tu método preferido.