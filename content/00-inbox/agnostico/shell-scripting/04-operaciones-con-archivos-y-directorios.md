# Operaciones con Archivos y Directorios en Bash

Uno de los usos más comunes de los scripts de shell es la gestión de archivos y directorios. Bash proporciona comandos integrados y utilidades del sistema para realizar prácticamente cualquier operación necesaria en el sistema de archivos.

## Verificación de Existencia

Antes de realizar operaciones en archivos o directorios, es importante verificar su existencia para evitar errores.

```bash
# Verificar si un directorio existe
if [ -d "$DIRECTORIO" ]; then
    echo "El directorio $DIRECTORIO existe"
else
    echo "El directorio $DIRECTORIO no existe"
fi

# Verificar si un archivo existe
if [ -f "$ARCHIVO" ]; then
    echo "El archivo $ARCHIVO existe"
else
    echo "El archivo $ARCHIVO no existe"
fi

# Otros tests útiles
# -e: existe (archivo o directorio)
# -r: tiene permiso de lectura
# -w: tiene permiso de escritura
# -x: tiene permiso de ejecución
# -s: existe y no está vacío
# -L: es un enlace simbólico
```

## Creación de Directorios

### mkdir
El comando `mkdir` crea directorios. La opción `-p` crea directorios padre según sea necesario.

```bash
# Crear un solo directorio
mkdir nuevo_directorio

# Crear directorios padre según sea necesario
mkdir -p ruta/al/nuevo/directorio

# Crear múltiples directorios
mkdir -p dir1/subdir1 dir2/subdir2 dir3
```

## Creación de Archivos

### touch
El comando `touch` actualiza la timestamp de un archivo o lo crea si no existe.

```bash
# Crear un archivo vacío o actualizar su timestamp
touch archivo.txt

# Crear múltiples archivos
touch archivo1.txt archivo2.txt archivo3.txt
```

### Redirección y Here Documents
También podemos crear archivos con contenido específico usando redirección o here documents.

```bash
# Crear archivo con contenido específico usando echo y redirección
echo "Este es el contenido del archivo" > archivo.txt

# Añadir contenido a un archivo existente
echo "Línea adicional" >> archivo.txt

# Crear archivo con múltiples líneas usando here document
cat > archivo.txt << EOF
Primera línea
Segunda línea
Tercera línea
EOF

# Crear archivo con contenido de una variable
echo "$CONTENIDO" > archivo.txt
```

## Lectura de Archivos

### Lectura Completa
```bash
# Mostrar todo el contenido de un archivo
cat archivo.txt

# Mostrar con números de línea
cat -n archivo.txt

# Mostrar sin buffering (útil para tuberías)
cat -u archivo.txt
```

### Lectura Línea por Línea
Para procesar archivos línea por línea, usamos un bucle while con read:

```bash
# Lectura básica línea por línea
while IFS= read -r linea; do
    echo "Línea leída: $linea"
done < archivo.txt

# Lectura que preserva espacios iniciales y finales (sin IFS=)
while read -r linea; do
    echo "Línea leída: '$linea'"
done < archivo.txt

# Lectura que divide en campos (similar a cut o awk)
while IFS=':' read -r usuario pwd uid gid comentario home shell; do
    echo "Usuario: $usuario, Home: $home, Shell: $shell"
done < /etc/passwd
```

## Escritura en Archivos

### Sobrescribir vs Anexar
```bash
# Sobrescribir el contenido (crea el archivo si no existe)
echo "Nuevo contenido" > archivo.txt

# Anexar al final del archivo (crea el archivo si no existe)
echo "Contenido adicional" >> archivo.txt

# Usando printf para mejor control de formato
printf "Nombre: %s\nEdad: %d\nCiudad: %s\n" "Juan" 30 "Madrid" > perfil.txt
```

### Escritura con Descriptores de Archivo
Para operaciones más avanzadas, podemos usar descriptores de archivo:

```bash
# Abrir archivo para escritura (descriptor 3)
exec 3> archivo_salida.txt

# Escribir en el descriptor 3
echo "Esta línea va al archivo 3" >&3
echo "Esta otra también" >&3

# Cerrar el descriptor cuando terminemos
exec 3>&-
```

## Búsqueda de Archivos

### find
El comando `find` es poderoso para buscar archivos según diversos criterios.

```bash
# Encontrar todos los archivos .txt en el directorio actual y subdirectorios
find . -type f -name "*.txt"

# Encontrar archivos modificados en los últimos 2 días
find . -type f -mtime -2

# Encontrar archivos más grandes de 10MB
find . -type f -size +10M

# Encontrar archivos vacíos
find . -type f -empty

# Ejecutar un comando en cada archivo encontrado
find . -type f -name "*.log" -exec grep "error" {} \;
# O de manera más eficiente con xargs
find . -type f -name "*.log" -print0 | xargs -0 grep "error"
```

### locate
Más rápido que find para búsquedas por nombre, pero depende de una base de datos actualizada.

```bash
# Actualizar la base de datos (requerido periódicamente)
sudo updatedb

# Buscar por nombre
locate *.conf
locate */bin/nginx
```

## Operaciones Básicas de Archivos

### Copiar (cp)
```bash
# Copiar un archivo
cp origen.txt destino.txt

# Copiar preservando atributos, modo, propietario y timestamps
cp -p origen.txt destino.txt

# Copiar directorios recursivamente
cp -r directorio_origen/ directorio_destino/

# Copiar mostrando progreso (requiere pv o similar)
cp -r origen/ destino/  # Para progreso avanzado: rsync -ah --progress origen/ destino/
```

### Mover/Renombrar (mv)
```bash
# Renombrar un archivo
mv nombre_viejo.txt nombre_nuevo.txt

# Mover un archivo a otro directorio
mv archivo.txt /nuevo/directorio/

# Mover preguntando antes de sobrescribir
mv -i archivo_viejo.txt destino/
```

### Eliminar (rm)
```bash
# Eliminar un archivo
rm archivo.txt

# Eliminar preguntando antes de cada eliminación
rm -i archivo1.txt archivo2.txt

# Eliminar directorios y su contenido recursivamente
rm -r directorio/

# Eliminar directorios vacíos
rmdir directorio_vacio

# Eliminar forzando sin preguntar (usar con extremo cuidado)
rm -rf /ruta/peligrosa
```

## Información de Archivos

### stat
Obtiene información detallada sobre un archivo o directorio.

```bash
# Información básica
stat archivo.txt

# Información específica usando formato personalizado
stat -c "%n %s %y" archivo.txt  # nombre, tamaño, fecha de modificación
stat -c "%U %G" archivo.txt     # propietario y grupo
stat -c "%a" archivo.txt        # permisos en formato octal
```

### file
Determina el tipo de archivo basado en su contenido (no en la extensión).

```bash
# Determinar tipo de archivo
file documento.pdf
file imagen.jpg
file ejecutable
file archivo_de_texto.txt

# Obtener solo el tipo (sin nombre de archivo)
file -b archivo.txt

# Obtener tipo MIME
file -i archivo.txt
```

## Ejemplos Prácticos Comunes

### Backup con Timestamp
```bash
#!/bin/bash
# Script simple de backup con timestamp

ORIGEN="/home/usuario/documentos"
DESTINO="/backups"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
NOMBRE_BACKUP="documentos_$TIMESTAMP.tar.gz"

# Crear directorio de destino si no existe
mkdir -p "$DESTINO"

# Crear el archivo tar comprimido
tar -czf "$DESTINO/$NOMBRE_BACKUP" -C "$(dirname "$ORIGEN")" "$(basename "$ORIGEN")"

echo "Backup creado: $DESTINO/$NOMBRE_BACKUP"
```

### Limpieza de Archivos Temporales
```bash
#!/bin/bash
# Elimina archivos temporales más antiguos de 7 días

DIR_TEMP="/tmp/miapp"
DIAS_ANTIGUOS=7

# Verificar que el directorio existe
if [ ! -d "$DIR_TEMP" ]; then
    echo "Directorio $DIR_TEMP no existe"
    exit 1
fi

# Encontrar y eliminar archivos antiguos
find "$DIR_TEMP" -type f -mtime +$DIAS_ANTIGUOS -delete

# También eliminar directorios vacíos
find "$DIR_TEMP" -type d -empty -delete

echo "Limpieza completada en $DIR_TEMP"
```

### Procesamiento de Logs
```bash
#!/bin/bash
# Analiza un log de acceso web y muestra las IPs más frecuentes

ARCHIVO_LOG="/var/log/nginx/access.log"

if [ ! -f "$ARCHIVO_LOG" ]; then
    echo "Archivo de log no encontrado: $ARCHIVO_LOG"
    exit 1
fi

# Extraer IPs, contar ocurrencias y ordenar por frecuencia
awk '{print $1}' "$ARCHIVO_LOG" | sort | uniq -c | sort -nr | head -10
```

## Buenas Prácticas

1. **Siempre usa comillas** alrededor de variables que contengan rutas de archivos:
   ```bash
   # Peligroso si la ruta contiene espacios
   rm $ARCHIVO
   
   # Seguro
   rm "$ARCHIVO"
   ```

2. **Verifica operaciones críticas** antes de ejecutarlas:
   ```bash
   # En lugar de hacer rm -r directamente
   if [ -d "$DIRECTORIO" ] && [ "$DIRECTORIO" != "/" ] && [ "$DIRECTORIO" != "" ]; then
       rm -r "$DIRECTORIO"
   else
       echo "Operación abortada por seguridad"
       exit 1
   fi
   ```

3. **Usa rutas absolutas en scripts** cuando sea posible para evitar ambigüedades.

4. **Maneja errores apropiadamente** verificando códigos de retorno:
   ```bash
   if ! mkdir -p "$DIRECTORIO"; then
       echo "Error: No se pudo crear el directorio $DIRECTORIO"
       exit 1
   fi
   ```

5. **Considera usar `rsync` en lugar de `cp -r`** para copias más robustas y con capacidad de reanudación.

6. **Para operaciones masivas**, considera procesar en lotes para evitar problemas con "Argument list too long".

## Ejercicios

1. Crea un script que haga un backup rotativo de un directorio (mantener las últimas 7 copias).
2. Desarrolla un script que encuentre y elimine archivos duplicados en un directorio basado en su contenido (no en el nombre).
3. Implementa un script que monitoree un directorio y notifique cuando se añadan, modifiquen o eliminen archivos.
4. Crea un script que reorganice archivos en subdirectorios basado en su extensión o fecha de creación.