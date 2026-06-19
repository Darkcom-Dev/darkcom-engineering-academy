# Comunicación entre Scripts en Bash

En entornos de scripting más complejos, es común necesitar compartir datos, configuraciones o funcionalidades entre múltiples scripts. Bash proporciona varios mecanismos para lograr esta comunicación, siendo el más común el uso de `source` (o su equivalente `.`) para importar variables y funciones de otros scripts.

## Sourcing de Scripts

El comando `source` (o su alias `.`) ejecuta el contenido de otro script en el contexto actual del shell, lo que significa que cualquier variable, función o alias definido en el script fuente quedará disponible en el script actual.

### Sintaxis Básica

```bash
# Usando source
source archivo.sh

# Usando el punto (equivalente)
. archivo.sh
```

### Diferencia entre source y ejecución normal

Cuando ejecutas un script normalmente (`./script.sh`), se crea un nuevo proceso shell, por lo que las variables y funciones definidas dentro de ese script no están disponibles en el shell padre después de que termine.

En cambio, cuando usas `source script.sh`, el contenido del script se ejecuta en el shell actual, por lo que todo lo que defina permanece disponible.

```bash
# archivo.sh
MI_VARIABLE="Hola desde el otro script"
mi_funcion() {
    echo "Esta es una función del otro script"
}

# script_principal.sh
#!/bin/bash
# Este enfoque NO funcionaría:
# ./archivo.sh
# echo $MI_VARIABLE  # Esto estaría vacío
# mi_funcion         # Esto fallaría

# Este enfoque SÍ funcionará:
source ./archivo.sh
echo $MI_VARIABLE  # Resultado: Hola desde el otro script
mi_funcion         # Resultado: Esta es una función del otro script
```

## Variables de Entorno vs Variables de Script

Es importante distinguir entre variables de entorno y variables de script:

### Variables de Entorno
Están disponibles para todos los procesos hijos y se heredan por defecto.

```bash
# Establecer una variable de entorno
export MI_VARIABLE="Valor disponible para hijos"

# En un script hijo:
#!/bin/bash
echo $MI_VARIABLE  # Esto funcionará porque es una variable de entorno
```

### Variables de Script
Solo están disponibles en el shell actual donde se definen, a menos que se usen `source`.

```bash
# En script_padre.sh
#!/bin/bash
MI_VARIABLE="Solo disponible aquí"
source ./script_hijo.sh  # El hijo puede acceder a esta variable
```

## Buenas Prácticas para Compartir Configuración

### Archivos de Configuración Comunes
Una práctica común es crear archivos de configuración que contengan variables que múltiples scripts pueden compartir.

```bash
# config.sh
# Configuración de la aplicación
DB_HOST="localhost"
DB_PORT="5432"
DB_NAME="miapp"
DB_USER="appuser"
DB_PASSWORD="secreto"

# Ruta de logs
LOG_DIR="/var/log/miapp"
LOG_LEVEL="INFO"

# Otras configuraciones
MAX_REINTENTOS=3
TIMEOUT_SEGUNDOS=30
```

Luego, otros scripts pueden simplemente hacer `source config.sh` para acceder a todas estas configuraciones:

```bash
#!/bin/bash
# backup_db.sh
source ./config.sh

# Ahora podemos usar todas las variables de configuración
echo "Conectando a $DB_HOST:$DB_PORT como $DB_USER..."
# ... resto del script de backup
```

### Evitar la Contaminación del Espacio de Nombres
Al sourcing de múltiples archivos, existe el riesgo de sobrescritura accidental de variables. Algunas estrategias para mitigarlo:

1. **Usar prefijos únicos** para variables de configuración:
   ```bash
   # En config.sh
   APP_DB_HOST="localhost"
   APP_DB_PORT="5432"
   APP_LOG_DIR="/var/log/miapp"
   ```

2. **Encapsular en funciones** cuando sea posible:
   ```bash
   # En lugar de variables globales, usar funciones de acceso
   get_db_host() {
       echo "localhost"
   }
   
   get_db_port() {
       echo "5432"
   }
   ```

3. **Usar espacios de nombres mediante variables asociativas** (bash 4.0+):
   ```bash
   # En config.sh
   declare -A CONFIG
   CONFIG[db_host]="localhost"
   CONFIG[db_port]="5432"
   CONFIG[log_dir]="/var/log/miapp"
   
   # En otros scripts
   source ./config.sh
   echo "Host: ${CONFIG[db_host]}"
   echo "Puerto: ${CONFIG[db_port]}"
   ```

## Parámetros y Variables de Posición

Cuando un script es sourced, hereda los mismos parámetros de posición que el script que lo llamó:

```bash
# parent.sh
#!/bin/bash
source ./child.sh "arg1" "arg2"

# child.sh
#!/bin/bash
echo "Primer argumento: $1"    # Resultado: arg1
echo "Segundo argumento: $2"   # Resultado: arg2
echo "Todos los argumentos: $@" # Resultado: arg1 arg2
```

## Exportando Variables para Subprocesos

Mientras que `source` comparte variables en el mismo shell, `export` hace que las variables estén disponibles para subprocesos (procesos hijos):

```bash
#!/bin/bash
# parent.sh

# Variable disponible solo en este shell (no para hijos)
LOCAL_VAR="Solo para este shell"

# Variable disponible para este shell y todos sus hijos
export GLOBAL_VAR="Para mí y mis hijos"

# Llamar a un script hijo
./child.sh

# Dentro de child.sh:
#!/bin/bash
echo "LOCAL_VAR: $LOCAL_VAR"      # Esto estará vacío
echo "GLOBAL_VAR: $GLOBAL_VAR"   # Esto mostrará "Para mí y mis hijos"
```

## Modularización de Funciones

Una práctica avanzada es crear bibliotecas de funciones que puedan ser reutilizadas entre múltiples scripts:

```bash
# librería.sh
#!/bin/bash

# Biblioteca de funciones para manejo de archivos
backup_file() {
    local archivo="$1"
    local directorio_backup="${2:-./backups}"
    
    mkdir -p "$directorio_backup"
    local timestamp=$(date +"%Y%m%d_%H%M%S")
    local nombre_backup="${directorio_backup}/$(basename "${archivo}")_${timestamp}.bak"
    
    cp "$archivo" "$nombre_backup"
    echo "$nombre_backup"
}

ensure_dir() {
    local dir="$1"
    if [ ! -d "$dir" ]; then
        mkdir -p "$dir"
        echo "Directorio creado: $dir"
    fi
}

log_message() {
    local nivel="$1"
    local mensaje="$2"
    local timestamp=$(date +"%Y-%m-%d %H:%M:%S")
    echo "[$timestamp] [$nivel] $mensaje"
}
```

Luego, otros scripts pueden usar esta biblioteca:

```bash
#!/bin/bash
# script_que_usa_la_libreria.sh
source ./librería.sh

# Usar las funciones de la biblioteca
log_message "INFO" "Iniciando proceso de backup"
ensure_dir "./backups"
archivo_backup=$(backup_file "./documentos/importante.txt")
log_message "INFO" "Backup creado en: $archivo_backup"
```

## Buenas Prácticas

1. **Usa nombres descriptivos** para los archivos que se van a source:
   ```bash
   # Bueno
   source ./configuracion_base_de_datos.sh
   source ./funciones_de_logging.sh
   
   # Menos bueno
   source ./config1.sh
   source ./helper.sh
   ```

2. **Documenta claramente** qué variables y funciones proporciona cada archivo sourced:
   ```bash
   # config.sh
   # Proporciona:
   #   DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASSWORD
   #   LOG_DIR, LOG_LEVEL
   ```

3. **Evita efectos secundarios** en los archivos sourced cuando sea posible:
   ```bash
   # Mal: el archivo sourced ejecuta código inmediatamente al ser sourceado
   # ¡Esto podría tener consecuencias no deseadas!
   
   # Bueno: el archivo solo define variables y funciones
   # El código que las usa va en el script principal
   ```

4. **Considera usar entornos virtuales o contenedores** para aplicaciones más complejas en lugar de depender únicamente de sourcing de scripts.

5. **Verifica que el archivo existe** antes de intentar sourcearlo:
   ```bash
   if [ -f "./config.sh" ]; then
       source ./config.sh
   else
       echo "Error: No se encontró el archivo de configuración config.sh"
       exit 1
   fi
   ```

## Ejercicios

1. Crea un archivo de configuración para una aplicación web que incluya variables para la base de datos, puertos de servicio, y rutas de logs. Luego crea dos scripts que usen esta configuración: uno para iniciar el servicio y otro para realizar backups.

2. Desarrolla una biblioteca de funciones para manejo de fechas y tiempos que incluya funciones para:
   - Convertir entre diferentes formatos de fecha
   - Calcular diferencias entre fechas
   - Generar timestamps para nombres de archivo
   - Luego crea un script que use esta biblioteca para generar reportes con timestamps en el nombre.

3. Implementa un sistema de plugins donde cada plugin es un script que define ciertas funciones, y un script principal que pueda cargar y usar estos plugins dinámicamente.

4. Crea un script que verifique la integridad de un conjunto de scripts fuentes mediante el uso de checksums o firmas digitales antes de sourcearlos.