# Gestión de Archivos y Sistema de Archivos en Linux

Esta categoría agrupa los comandos esenciales para trabajar con archivos, directorios y el sistema de archivos en sistemas Linux. Incluye herramientas para crear, modificar, comprimir, verificar y gestionar el almacenamiento de datos.

## Comandos de Archivos y Directorios

| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| **ls** | Lista el contenido de directorios | `ls -la` |
| **cd** | Cambia de directorio | `cd /ruta/al/directorio` |
| **pwd** | Muestra el directorio de trabajo actual | `pwd` |
| **mkdir** | Crea directorios | `mkdir -p directorio/subdir` |
| **rm** | Elimina archivos y directorios | `rm -rf directorio` (usar con cuidado) |
| **cp** | Copia archivos y directorios | `cp -r origen destino` |
| **mv** | Mueve o renombra archivos y directorios | `mv archivo nuevo_nombre` |
| **touch** | Crea archivos vacíos o actualiza timestamps | `touch archivo.txt` |
| **find** | Busca archivos en el sistema de archivos | `find / -name "*.conf"` |
| **locate** | Busca archivos usando una base de datos indexada | `locate nombre_archivo` |

## Visualización y Edición de Archivos

| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| **cat** | Concatena y muestra el contenido de archivos | `cat archivo.txt` |
| **tac** | Muestra archivos en orden inverso | `tac archivo.txt` |
| **less** | Visor de archivos paginado (ideal para archivos grandes) | `less archivo.log` |
| **head** | Muestra las primeras líneas de un archivo | `head -20 archivo.txt` |
| **tail** | Muestra las últimas líneas de un archivo | `tail -f archivo.log` (seguimiento en tiempo real) |
| **more** | Visor de archivos paginado básico | `more archivo.txt` |
| **nl** | Numera líneas de archivos | `nl archivo.txt` |
| **od** | Vuelve a octal (y otros formatos) el contenido de archivos | `od -c archivo.bin` |

## Compresión y Archivado

| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| **tar** | Archiva múltiples archivos en un solo archivo (tape archive) | `tar -czf archivo.tar.gz directorio/` |
| **gzip** | Comprime archivos usando algoritmo LZ77 | `gzip archivo.txt` |
| **gunzip** | Descomprime archivos .gz | `gunzip archivo.gz` |
| **zip** | Crea archivos ZIP (formato multi-plataforma) | `zip -r archivo.zip directorio/` |
| **unzip** | Extrae archivos ZIP | `unzip archivo.zip` |
| **bzip2** | Comprime archivos usando algoritmo Burrows-Wheeler | `bzip2 archivo.txt` |
| **xz** | Comprime archivos con alta relación de compresión | `xz -9 archivo.txt` |
| **rar** | Crea y extrae archivos RAR (requiere instalación separada) | `rar a archivo.rar directorio/` |

## Permisos y Propiedad

| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| **chmod** | Cambia permisos de archivos y directorios | `chmod 755 archivo.sh` |
| **chown** | Cambia el propietario y grupo de archivos | `chown usuario:grupo archivo` |
| **chgrp** | Cambia únicamente el grupo propietario | `chgrp grupo archivo` |
| **umask** | Establece los permisos por defecto para nuevos archivos | `umask 022` |

## Sistema de Archivos y Discos

| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| **df** | Muestra el uso de espacio en sistemas de archivos montados | `df -h` |
| **du** | Estima el uso de espacio en archivos y directorios | `du -sh *` |
| **fdisk** | Manipula tablas de particiones de discos | `sudo fdisk /dev/sda` |
| **mkfs** | Crea sistemas de archivos en particiones | `sudo mkfs.ext4 /dev/sda1` |
| **fsck** | Verifica y repara sistemas de archivos | `sudo fsck /dev/sda1` |
| **mount** | Monta sistemas de archivos en puntos de montaje | `sudo mount /dev/sda1 /mnt` |
| **umount** | Desmonta sistemas de archivos | `sudo umount /mnt` |
| **blkid** | Localiza y muestra atributos de dispositivos de bloque | `blkid` |
| **lsblk** | Lista información de dispositivos de bloque | `lsblk -f` |

## Herramientas de Búsqueda y Procesamiento de Texto

| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| **grep** | Busca patrones de texto dentro de archivos | `grep -r "patrón" /directorio` |
| **sed** | Editor de flujos para transformar texto | `sed 's/antiguo/nuevo/g' archivo` |
| **awk** | Lenguaje de procesamiento de patrones y campos | `awk -F: '{print $1}' /etc/passwd` |
| **cut** | Extrae secciones específicas de líneas de archivos | `cut -d: -f1 /etc/passwd` |
| **sort** | Ordena líneas de archivos de texto | `sort -n archivo.txt` |
| **uniq** | Reporte o omite líneas repetidas | `sort archivo | uniq -c` |
| **diff** | Compara diferencias entre archivos | `diff archivo1.txt archivo2.txt` |
| **patch** | Aplica parches a archivos originales | `patch < cambios.patch` |
| **join** | Une líneas de dos archivos basado en un campo común | `join -t: archivo1.txt archivo2.txt` |

## Herramientas de Miscellanées Útiles

| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| **echo** | Muestra texto o variables en pantalla | `echo "Hola Mundo"` |
| **printf** | Formatea y muestra datos (más preciso que echo) | `printf "Nombre: %s\\nEdad: %d\\n" "Juan" 30` |
| **basename** | Extrae el nombre de archivo de una ruta completa | `basename /ruta/archivo.txt` |
| **dirname** | Extrae el directorio de una ruta completa | `dirname /ruta/archivo.txt` |
| **readlink** | Resuelve enlaces simbólicos | `readlink -f /enlace/simbólico` |
| **file** | Determina el tipo de archivo basado en su contenido | `file archivo_desconocido` |
| **stat** | Muestra información detallada de archivos y sistemas de archivos | `stat archivo.txt` |
| **seq** | Genera secuencias de números | `seq 1 10` |
| **bc** | Calculadora de precisión arbitraria | `echo "scale=4; 22/7" | bc` |
| **jq** | Procesador de JSON para línea de comandos | `jq '.nombre' datos.json` |
| **yaml** | Procesador de YAML para línea de comandos | `yaml parse config.yaml` |

## Buenas Prácticas

1. **Siempre verifica antes de eliminar**: Usa `ls` o `ls -la` antes de ejecutar comandos `rm` potencialmente destructivos.
2. **Usa comillas alrededor de variables** cuando trabajes con rutas que puedan contener espacios.
3. **Prefiere rutas absolutas en scripts** para evitar ambigüedades.
4. **Verifica el espacio disponible** antes de operaciones de escritura masiva con `df -h`.
5. **Haz backups** antes de modificar configuraciones críticas del sistema.
6. **Usa `sudo` solo cuando sea necesario** y nunca en scripts que se ejecuten automáticamente sin supervisión.
7. **Aprende los permisos básicos**: 644 para archivos normales, 755 para ejecutables y directorios.
8. **Comprueba el tipo de archivo** con `file` antes de intentar procesarlo como texto o ejecutarlo.

> **Nota**: Esta lista representa solo una selección de los comandos más utilizados para gestión de archivos en Linux. Cada comando tiene múltiples opciones y casos de uso avanzados que pueden explorarse consultando sus respectivas páginas de manual (`man comando`).