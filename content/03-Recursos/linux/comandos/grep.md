# Grep: Búsqueda de Patrones en Archivos

`grep` es uno de los comandos más poderosos y esenciales en el entorno de línea de comandos de Linux. Permite buscar patrones específicos dentro de archivos utilizando expresiones regulares, lo que lo convierte en una herramienta indispensable para administradores de sistemas, desarrolladores y cualquier persona que trabaje con datos de texto.

## Sintaxis Básica

```bash
grep [opciones] patrón [archivo...]
```

- **patrón**: La cadena de texto o expresión regular que deseas buscar
- **archivo**: Uno o más archivos en los que realizar la búsqueda (si se omite, lee de la entrada estándar)

## Características Principales

- Búsqueda sensible a mayúsculas/minúsculas por defecto (pero se puede hacer insensible)
- Soporte para expresiones regulares básicas, extendidas y compatibles con Perl
- Capacidad para buscar en múltiples archivos simultáneamente
- Salida personalizable con números de línea, nombres de archivo, contexto, etc.
- Opciones para invertir la búsqueda, contar coincidencias, mostrar solo partes específicas, etc.

## Opciones Más Utilizadas

### Control de Sensibilidad a Mayúsculas
| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-i` | Ignora diferencias entre mayúsculas y minúsculas | `grep -i "error" log.txt` |
| `--no-ignore-case` | Forza sensibilidad a mayúsculas/minúsculas (por defecto) | - |

### Control de Salida
| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-n` | Muestra el número de línea junto con cada coincidencia | `grep -n "función" script.py` |
| `-c` | Solo muestra el conteo de líneas que coinciden | `grep -c "OK" resultados.txt` |
| `-v` | Invierte la búsqueda (muestra líneas que NO coinciden) | `grep -v "^#" configuración.conf` |
| `-l` | Lista solo los nombres de archivos que contienen coincidencias | `grep -l "TODO" *.md` |
| `-L` | Lista solo los nombres de archivos que NO contienen coincidencias | `grep -L "licencia" *.txt` |
| `-o` | Muestra solo la parte que coincide, no toda la línea | `grep -o "[0-9]\+" datos.txt` |

### Control de Contexto
Estas opciones muestran líneas adicionales alrededor de las coincidencias:
| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-A N` | Muestra N líneas después de cada coincidencia (después) | `grep -A 3 "error" log.txt` |
| `-B N` | Muestra N líneas antes de cada coincidencia (antes) | `grep -B 2 "falló" log.txt` |
| `-C N` | Muestra N líneas antes y después de cada coincidencia (contexto) | `grep -C 1 "puntuación máxima" juego.log` |

### Búsqueda en Múltiples Archivos y Directorios
| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-r` o `-R` | Busca recursivamente en directorios | `grep -r "configuración" /etc/` |
| `-h` | Suprime el prefijo del nombre de archivo en la salida | `grep -h "main" *.c` |
| `-H` | Fuerza la inclusión del nombre de archivo (útil con un solo archivo) | `grep -H "error" único.log` |
| `--include=GLOB` | Busca solo archivos que coincidan con el patrón GLOB | `grep --include="*.py" -r "import" proyecto/` |
| `--exclude=GLOB` | Omite archivos que coincidan con el patrón GLOB | `grep --exclude="*.log" -r "DEBUG" .` |

### Expresiones Regulares
| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-E` | Usa expresiones regulares extendidas | `grep -E "error|falló" log.txt` |
| `-F` | Trata el patrón como una cadena fija (no como regex) | `grep -F ".conf" archivos.txt` |
| `-P` | Usa expresiones regulares compatibles con Perl (experimental) | `grep -P "\d{3}-\d{2}-\d{4}" datos.txt` |

## Ejemplos Prácticos

### Búsqueda Simple
```bash
# Busca la palabra "error" en un archivo de log
grep "error" application.log

# Búsqueda insensible a mayúsculas/minúsculas
grep -i "error" application.log
```

### Trabajar con Múltiples Archivos
```bash
# Busca en todos los archivos .txt del directorio actual
grep "importante" *.txt

# Busca recursivamente en todo el directorio actual y subdirectorios
grep -r "función" .

# Muestra solo los nombres de archivos que contienen la palabra "LICENCIA"
grep -l "LICENCIA" *.txt
```

### Usando Expresiones Regulares
```bash
# Busca líneas que comiencen con "ERROR" o "ADVERTENCIA"
grep -E "^(ERROR|ADVERTENCIA)" log.txt

# Busca direcciones IP simples (formato xxx.xxx.xxx.xxx)
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" acceso.log

# Busca líneas que contengan exactamente 4 dígitos consecutivos
grep -E "[0-9]{4}" datos.txt
```

### Filtrado y Procesamiento
```bash
# Muestra el número de líneas que contienen "usuario" en el archivo de passwd
grep -c "usuario" /etc/passwd

# Muestra las líneas que NO comienzan con # (comentarios)
grep -v "^#" configuración.ini

# Obtiene solo las direcciones IP de un log de acceso
grep -oE "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" acceso.log | sort -u
```

## Consejos y Buenas Prácticas

1. **Siempre usa comillas** alrededor de tu patrón para evitar que el shell interprete caracteres especiales
   ```bash
   # Correcto
   grep "hora\|fecha" log.txt
   
   # Incorrecto (el shell intentará ejecutar "hora" como comando)
   grep hora|fecha log.txt
   ```

2. **Combina con otros comandos** usando tuberías para filtrado avanzado
   ```bash
   # Encuentra archivos grandes y busca en ellos patrones específicos
   find . -type f -size +1M -exec grep -l "error" {} \;
   
   # O más eficientemente con xargs
   find . -type f -size +1M -print0 | xargs -0 grep -l "error"
   ```

3. **Usa el contexto** para entender mejor las coincidencias
   ```bash
   # Al depurar, ver 2 líneas antes y después te da contexto valioso
   grep -C 2 "Exception" aplicación.log
   ```

4. **Para búsquedas muy grandes**, considera usar `-m` para limitar resultados
   ```bash
   # Solo muestra las primeras 5 coincidencias
   grep -m 5 "patrón" archivo_grande.txt
   ```

## Recursos Adicionales

- Para ver el manual completo: `man grep`
- Para ejemplos adicionales: `info grep`
- Tutorial interactivo: prueba buscar patrones en `/usr/share/dict/words` o en los logs de tu sistema

> **Nota**: Aunque este documento cubre las características más importantes de `grep`, el comando tiene muchas más opciones y capacidades. La práctica constante es la mejor manera de dominar su uso efectivo.