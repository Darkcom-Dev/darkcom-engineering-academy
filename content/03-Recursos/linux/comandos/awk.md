# Awk: Procesamiento de Texto y Extracción de Datos

`awk` es un lenguaje de programación poderoso diseñado específicamente para el procesamiento de patrones y el procesamiento de texto. Permite extraer, transformar y analizar datos estructurados en archivos o flujos de entrada, realizando operaciones en columnas y campos de manera eficiente.

## ¿Qué es Awk?

Awk toma su nombre de las iniciales de sus creadores: **A**ho, **W**einberger y **K**ernighan. Opera leyendo entrada línea por línea, dividiendo cada línea en campos (por defecto separados por espacios en blanco o tabulaciones) y aplicando acciones especificada por el usuario.

## Sintaxis Básica

```bash
awk [opciones] 'patrón { acción }' [archivo...]
```

- **patrón**: Condición que determina cuándo ejecutar la acción (opcional)
- **acción**: Comandos a ejecutar cuando se cumple el patrón (entre llaves)
- **archivo**: Uno o más archivos de entrada (si se omite, lee de la entrada estándar)

## Campos y Variables Integradas

Awk divide automáticamente cada línea de entrada en campos:

| Variable | Descripción |
|----------|-------------|
| `$0` | La línea completa de entrada |
| `$1`, `$2`, ..., `$n` | El primer, segundo, ..., n-ésimo campo |
| `NF` | Número de campos en la línea actual |
| `NR` | Número total de registros (líneas) leídas hasta ahora |
| `FNR` | Número de registros en el archivo actual (se reinicia con cada nuevo archivo) |
| `FS` | Separador de campo (por defecto: espacios en blanco o tabulaciones) |
| `OFS` | Separador de campo de salida (por defecto: espacio en blanco) |
| `RS` | Separador de registro (por defecto: nueva línea) |
| `ORS` | Separador de registro de salida (por defecto: nueva línea) |
| `FILENAME` | Nombre del archivo de entrada actual |

## Opciones Más Utilizadas

| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-F` | Especifica el separador de campo | `awk -F: '{print $1}' /etc/passwd` |
| `-v` | Asigna una variable antes de la ejecución | `awk -v umbral=100 '$2 > umbral' datos.txt` |
| `-f` | Lee el programa awk desde un archivo | `awk -f script.awk datos.txt` |
| `--help` | Muestra ayuda y sale | `awk --help` |

## Ejemplos Prácticos

### Extracción Simple de Columnas
```bash
# Muestra solo la primera columna (permisos) de ls -l
ls -l | awk '{print $1}'

# Muestra la quinta columna (tamaño) y la novena (nombre) de ls -l
ls -l | awk '{print $5, $9}'

# Muestra usuario y directorio home de /etc/passwd (usando : como separador)
awk -F: '{print $1, $6}' /etc/passwd
```

### Operaciones Matemáticas
```bash
# Calcula el tamaño en KiB de cada archivo (columna 5 de ls -l)
ls -l | awk '{print $5/1024 " KiB", $9}'

# Suma dos números de entrada
echo "11 22" | awk '{print $1 + $2}'

# Calcula la suma de todos los números en una línea
echo "10 20 30 40" | awk '{ 
    suma = 0
    for(i = 1; i <= NF; i++) {
        suma += $i
    }
    print "Suma:", suma
}'

# Calcula el promedio de los números en una línea
echo "10 20 30 40" | awk '{ 
    suma = 0
    for(i = 1; i <= NF; i++) {
        suma += $i
    }
    print "Promedio:", suma/NF
}'
```

### Patrón y Acción
```bash
# Muestra líneas donde el tercer campo es mayor que 100
awk '$3 > 100' datos.txt

# Muestra solo el primer campo de líneas donde el segundo campo contiene "activo"
awk '$2 ~ /activo/ {print $1}' estados.txt

# Muestra líneas que NO comienzan con # (comentarios)
awk '!/^#/' configuracion.conf

# Muestra el número de línea y la línea misma para líneas que contienen "error"
awk '/error/ {print NR ": " $0}' log.txt
```

### Bloques BEGIN y END
```bash
# Añade un encabezado y pie de página a la salida de ls -l
ls -l | awk '
    BEGIN { 
        print "Lista de archivos con sus tamaños:"
        print "--------------------------------"
    }
    {
        printf "%-10s %s\n", $5/1024 "KiB", $9
    }
    END {
        print "--------------------------------"
        print "Total de archivos procesados: " NR
    }
' 

# Calcula el total de tamaños de archivos
ls -l | awk '
    BEGIN { total = 0 }
    { total += $5 }
    END { 
        print "Tamaño total: " total/1024/1024 " MB" 
    }
'
```

### Manipulación de Cadenas
```bash
# Concatena campos sin espacios
echo "Hola mundo" | awk '{print $1 $2}'  # Resultado: Holamundo

# Concatena campos con espacios
echo "Hola mundo" | awk '{print $1, $2}'  # Resultado: Hola mundo

# Extrae partes específicas de una cadena
echo "Juan Pérez: 30 años, Desarrollador" | awk -F': ' '{print $1}'  # Resultado: Juan Pérez

# Divide una fecha y muestra componentes
echo "2023-12-25" | awk -F'-' '{print "Día: " $3 ", Mes: " $2 ", Año: " $1}'
```

### Trabajando con Múltiples Archivos
```bash
# Procesa múltiples archivos y muestra qué archivo está procesando actualmente
awk '{
    print FILENAME ": línea " FNR ": " $0
}' archivo1.txt archivo2.txt

# Reinicia el conteo de líneas en cada archivo
awk '{print FNR ": " $0}' *.log

# Número absoluto de línea a través de múltiples archivos
awk '{print NR ": " $0}' *.log
```

## Funciones Integradas Útiles

Awk incluye muchas funciones integradas para manipulación de cadenas, matemáticas, etc.:

| Función | Descripción | Ejemplo |
|---------|-------------|---------|
| `length(str)` | Devuelve la longitud de una cadena | `awk '{print length($0)}'` |
| `tolower(str)` | Convierte a minúsculas | `awk '{print tolower($0)}'` |
| `toupper(str)` | Convierte a mayúsculas | `awk '{print toupper($0)}'` |
| `substr(str, inicio, longitud)` | Extrae subcadena | `awk '{print substr($1, 1, 3)}'` |
| `index(str, buscar)` | Encuentra posición de substring | `awk '{print index($0, "error")}'` |
| `match(str, regex)` | Encuentra posición de coincidencia regex | `awk '{print match($0, /[0-9]+/)}'` |
| `split(str, array, separador)` | Divide cadena en array | `awk '{split($0, partes, "-"); print partes[1]}'` |
| `sin(x), cos(x), sqrt(x), etc.` | Funciones trigonométricas y matemáticas | `awk '{print sqrt($1)}'` |
| `int(x)` | Parte entera de un número | `awk '{print int($1)}'` |
| `rand()` | Número aleatorio entre 0 y 1 | `awk '{print rand()}'` |
| `srand()` | Inicializa generador de números aleatorios | `BEGIN { srand() }` |

## Ejemplos Avanzados

### Contador de Palabras Simplificado
```bash
# Cuenta palabras en cada línea
awk '{ 
    palabras = 0
    for(i = 1; i <= NF; i++) {
        if($i != "") palabras++
    }
    print NR ": " palabras " palabras"
}' texto.txt

# O más simplemente (NF ya cuenta campos separados por espacios)
awk '{print NR ": " NF " palabras"}' texto.txt
```

### Filtrado y Transformación de CSV
```bash
# Asumiendo archivo CSV con encabezado: nombre,edad,ciudad
awk -F, '
    NR == 1 { 
        print "Nombre\tEdad\tCiudad"  # Encabezado formateado
        next  # Saltar procesamiento adicional para la primera línea
    }
    $2 >= 18 {  # Solo mayores de edad
        print $1 "\t" $2 "\t" $3
    }
    END {
        print "Total personas mayores de edad: " count
    }
' datos.csv
```

### Reporte de Log de Acceso Web
```bash
# Procesar log de formato común: IP - - [fecha] "método ruta proto" estado tamaño
awk '
{
    ip = $1
    fecha_hora = $4 " " $5
    metodo_ruta_proto = $6
    codigo = $9
    tamaño = $10
    
    # Contar por código de estado
    codigos[codigo]++
    
    # Sumar total de bytes transferidos
    if (tamaño != "-") {
        total_bytes += tamaño
    }
    
    # Guardar última IP para reporte
    ultima_ip = ip
}
END {
    print "=== Estadísticas del Log ==="
    print "Última IP procesada: " ultima_ip
    print ""
    print "Distribución de códigos de estado:"
    for (codigo in codigos) {
        printf "  %s: %d peticiones\n", codigo, codigos[codigo]
    }
    print ""
    print "Total bytes transferidos: " total_bytes
}
' acceso.log
```

## Consejos y Buenas Prácticas

1. **Usa comillas simples** alrededor del programa awk para evitar que el shell interprete variables y caracteres especiales
   ```bash
   # Correcto
   awk '{print $1}' archivo.txt
   
   # Problemático si $1 es una variable de shell
   awk "{print $1}" archivo.txt
   ```

2. **Especifica explícitamente el separador de campo** cuando trabajes con formatos conocidos
   ```bash
   # Mejor que confiar en el predeterminado para archivos con espacios en campos
   awk -F: '{print $1}' /etc/passwd
   ```

3. **Utiliza patrones para filtrado eficiente** en lugar de if dentro de acciones cuando sea posible
   ```bash
   # Más eficiente
   awk '$3 > 100 {print $0}' datos.txt
   
   # Menos eficiente (pero equivalente)
   awk '{if ($3 > 100) print $0}' datos.txt
   ```

4. **Inicializa variables en BEGIN** para asegurar valores conocidos
   ```bash
   awk '
       BEGIN { suma = 0; contador = 0 }
       { suma += $1; contador++ }
       END { print "Promedio: " (suma/contador) }
   ' numeros.txt
   ```

5. **Combina con otros comandos** mediante tuberías para flujos de trabajo complejos
   ```bash
   # Encuentra archivos modificados recientemente y muestra sus tamaños legibles
   find . -type f -mtime -1 -exec ls -lh {} \; | awk '{print $5, $9}'
   
   # O usando print0 y xargs para manejar nombres con espacios
   find . -type f -mtime -1 -print0 | xargs -0 ls -lh | awk '{print $5, $9}'
   ```

## Recursos Adicionales

- Para ver el manual completo: `man awk` o `man gawk`
- Para información detallada: `info awk`
- El libro clásico: "The AWK Programming Language" por Aho, Weinberger y Kernighan
- Versión GNU (gawk) tiene extensiones adicionales útiles para procesamiento avanzado

> **Nota**: Aunque este documento cubre las características más importantes y usos comunes de `awk`, el lenguaje es sorprendentemente poderoso y completo. Con práctica, puedes usar awk para tareas que van desde simples extracciones de columnas hasta análisis de datos complejos, generación de informes y hasta pequeños programas de procesamiento de texto sofisticados.