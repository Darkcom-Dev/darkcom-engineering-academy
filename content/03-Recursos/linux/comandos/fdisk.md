# Fdisk: Gestión de Particiones de Discos

`fdisk` es una herramienta de línea de comandos utilizada para crear, modificar, eliminar y gestionar tablas de particiones en discos duros y otros dispositivos de almacenamiento. Es una utilidad esencial para administradores de sistemas y usuarios avanzados que necesitan configurar el almacenamiento a nivel de particiones.

## ¿Qué es Fdisk?

Fdisk (format disk) es una aplicación de diálogo que permite manipular las tablas de particiones de discos. Soporta varios tipos de tablas de partición incluyendo:
- **MBR** (Master Boot Record) - El esquema tradicional de particionamiento
- **GPT** (GUID Partition Table) - El esquema moderno utilizado en sistemas UEFI
- Otros formatos menos comunes como Sun, SGI y BSD

> **Importante**: Modificar particiones puede resultar en pérdida de datos. Siempre respalde sus datos importantes antes de usar fdisk y asegúrese de estar trabajando en el dispositivo correcto.

## Sintaxis Básica

```bash
fdisk [opciones] dispositivo
```

- **dispositivo**: La ruta al dispositivo de bloque que se va a particionar (ej: /dev/sda, /dev/nvme0n1)
- **opciones**: Flags que modifican el comportamiento predeterminado de fdisk

## Iniciando Fdisk

Para comenzar a trabajar con un dispositivo, simplemente ejecuta:
```bash
sudo fdisk /dev/sda
```

Esto abrirá la interfaz interactiva de fdisk donde podrás ejecutar diversos comandos. Necesitas privilegios de administrador (sudo) para modificar particiones.

## Comandos Interactivos de Fdisk

Una vez dentro de fdisk, se presenta un prompt donde puedes ingresar comandos de un solo carácter. Aquí están los más importantes organizados por categoría:

### Gestión de Particiones
| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| `n` | **Crear una nueva partición** | Añade una partición nueva al disco |
| `d` | **Eliminar una partición** | Borra una partición existente (¡cuidado con los datos!) |
| `p` | **Mostrar la tabla de particiones** | Vista actual de todas las particiones en el disco |
| `t` | **Cambiar el tipo de una partición** | Modifica el código de tipo (ej: de Linux a swap) |
| `i` | **Imprimir información detallada** | Muestra datos específicos sobre una partición |

### Operaciones en la Tabla de Particiones
| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| `w` | **Escribir cambios y salir** | Guarda todas las modificaciones en el disco y sale |
| `q` | **Salir sin guardar** | Abandona fdisk sin aplicar cambios |
| `v` | **Verificar la tabla de particiones** | Revisa la consistencia de la tabla actual |

### Configuración y Opciones Avanzadas
| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| `a` | **Conmutar el indicador de arranque** | Marca/desmarca una partición como bootable (solo MBR) |
| `b` | **Modificar etiqueta de disco BSD anidada** | Para configuraciones específicas BSD |
| `c` | **Conmutar indicador de compatibilidad con DOS** | Ajusta compatibilidad con sistemas antiguos |
| `F` | **Listar espacio libre no particionado** | Muestra regiones sin particionar disponibles |
| `l` | **Listar tipos de particiones conocidos** | Muestra todos los códigos de tipo disponibles |
| `m` | **Mostrar este menú de ayuda** | Muestra la lista de comandos disponibles |
| `u` | **Cambiar unidades de visualización/entrada** | Alterna entre sectores y cilindros |
| `x` | **Funciones adicionales** | Menú experto para operaciones avanzadas |

### Operaciones de Script (No Interactivas)
| Comando | Descripción | Uso Común |
|---------|-------------|-----------|
| `I` | **Cargar estructura del disco desde archivo sfdisk** | Restaura particiones desde un guion |
| `O` | **Volcar estructura del disco a archivo sfdisk** | Crea un respaldo de la tabla de particiones |
| `g` | **Crear nueva tabla GPT vacía** | Inicializa disco con esquema GPT |
| `G` | **Crear nueva tabla SGI (IRIX) vacía** | Para sistemas SGI específicos |
| `o` | **Crear nueva tabla DOS (MBR) vacía** | Inicializa disco con esquema MBR |
| `s` | **Crear nueva tabla Sun vacía** | Para sistemas Sun/Sparc específicos |

## Ejemplos Prácticos

### Ver Particiones Existentes
```bash
# Muestra las particiones de todos los dispositivos
sudo fdisk -l

# Muestra las particiones de un dispositivo específico
sudo fdisk -l /dev/sdb
```

### Crear una Nueva Partición (Ejemplo Paso a Paso)
```bash
sudo fdisk /dev/sdc
```
Dentro de fdisk:
1. `p` - Ver particiones actuales
2. `n` - Nueva partición
   - Elige tipo: `p` para primaria o `l` para lógica (en MBR)
   - Número de partición (ej: 1)
   - Primer sector (presiona Enter para aceptar el predeterminado)
   - Último sector o tamaño (ej: +20G para 20 gigabytes)
3. `t` - Cambiar tipo (si es necesario)
   - Ingresa el número de partición
   - Ingresa el código hexadecimal (ej: 83 para Linux, 82 para swap)
   - O usa `L` para listar todos los tipos disponibles
4. `p` - Verificar la nueva tabla de particiones
5. `w` - Escribir cambios y salir

### Eliminar una Partición
```bash
sudo fdisk /dev/sdd
```
Dentro de fdisk:
1. `p` - Identificar número de partición a eliminar
2. `d` - Eliminar partición
   - Ingresa el número de partición a borrar
3. `p` - Verificar que se eliminó correctamente
4. `w` - Escribir cambios y salir

### Cambiar el Tipo de Partición
```bash
sudo fdisk /dev/sde
```
Dentro de fdisk:
1. `p` - Ver particiones y sus tipos actuales
2. `t` - Cambiar tipo
   - Ingresa el número de partición
   - Ingresa el código hexadecimal nuevo (o `L` para listar)
3. `p` - Verificar el cambio
4. `w` - Escribir cambios y salir

### Crear una Nueva Tabla de Particiones (Advertencia: Borra Todo)
```bash
# Para crear una tabla GPT vacía (borra todas las particiones existentes)
sudo fdisk /dev/sdf
```
Dentro de fdisk:
1. `g` - Crea nueva tabla GPT vacía
2. `p` - Verifica que la tabla esté vacía
3. Continúa creando particiones según necesites
4. `w` - Escribir cambios y salir

## Trabajando con Diferentes Esquemas de Particionamiento

### MBR (Master Boot Record)
- Límite de 2TB por disco
- Máximo 4 particiones primarias (o 3 primarias + 1 extendida con particiones lógicas)
- Compatible con BIOS tradicional y algunos sistemas UEFI en modo legacy
```bash
# Crear tabla MBR vacía
sudo fdisk /dev/sdX
# Dentro de fdisk:
o   # Nueva tabla DOS (MBR)
w   # Escribir y salir
```

### GPT (GUID Partition Table)
- Soporta discos mucho más grandes (hasta 9.4 ZB)
- Hasta 128 particiones por defecto en Linux
- Requiere UEFI para arranque, aunque algunos BIOS pueden leerlo
- Incluye valores de redundancia (cabecera y tabla al inicio y final del disco)
```bash
# Crear tabla GPT vacía
sudo fdisk /dev/sdX
# Dentro de fdisk:
g   # Nueva tabla GPT vacía
w   # Escribir y salir
```

## Consejos y Buenas Prácticas

1. **Siempre verifica el dispositivo correcto**:
   ```bash
   # Lista todos los dispositivos de bloque antes de operar
   lsblk
   # o
   sudo fdisk -l
   ```

2. **Haz una copia de seguridad de la tabla de particiones**:
   ```bash
   # Guarda la tabla actual antes de hacer cambios
   sudo sfdisk -d /dev/sda > respaldo_particiones.sfdisk
   ```

3. **Entiende la diferencia entre particiones primarias, extendidas y lógicas (MBR)**:
   - Primarias: Máximo 4, una puede marcarse como activa/bootable
   - Extendida: Contenedor especial que puede albergar múltiples particiones lógicas
   - Lógicas: Particiones dentro de una partición extendida

4. **Alinea las particiones correctamente para rendimiento**:
   - En discos modernos, alinea a múltiplos de 1MiB (generalmente predeterminado en fdisk reciente)
   - Evita empezar particiones en sectores que no sean múltiplos de 2048 (1MiB) en discos de 512B/sector

5. **Usa los códigos de tipo apropiados**:
   - 83: Linux filesystem
   - 82: Linux swap
   - ef: EFI System Partition (para UEFI)
   - fd: Linux RAID
   - 8e: Linux LVM

6. **Combina con otras herramientas para un flujo completo**:
   ```bash
   # Después de crear particiones con fdisk:
   sudo mkfs.ext4 /dev/sda1      # Formatea partición
   sudo mkswap /dev/sda2         # Configura swap
   sudo mount /dev/sda1 /mnt     # Monta partición
   ```

7. **Ten cuidado con discos que contienen datos importantes**:
   - Siempre verifica dos veces el dispositivo (`/dev/sda` vs `/dev/sdb`)
   - Considera usar `gdisk` o `parted` como alternativas más seguras para algunos casos
   - En sistemas de producción, prefiera herramientas con interfaces gráficas como GParted cuando sea posible

## Solución de Problemas Comunes

### "Dispositivo está ocupado"
```bash
# Identifica qué proceso está usando el dispositivo
sudo lsof /dev/sda1
# o
sudo fuser -v /dev/sda1

# Desmonta si es necesario
sudo umount /dev/sda1
```

### "Partition table entries are not in disk order"
Esto ocurre cuando las particiones no están secuencialmente ordenadas por número de sector.
- En fdisk moderno, usa la opción de orden experto (`x` entonces `f`) para corregir el orden
- O respalda los datos, elimina y recrea las particiones en el orden correcto

### No reconoce el tamaño completo del disco
- Asegúrate de no estar usando una versión muy antigua de fdisk
- Algunos discos muy nuevos pueden requerir utilidades específicas del fabricante
- Verifica que el controlador y el kernel reconozcan correctamente el tamaño

## Recursos Adicionales

- Para ver el manual completo: `man fdisk`
- Para información detallada: `info fdisk`
- Para discos GPT: considera también `gdisk` (parte del paquete gptfdisk)
- Para particionado más amigable: prueba `parted` o `gparted` (interfaz gráfica)
- Para documentación sobre esquemas de particionamiento: busca en la documentación de tu distribución sobre MBR vs GPT

> **Nota**: Aunque fdisk es poderoso y ampliamente disponible, para tareas de particionado complejas o cuando se trabaja con discos muy grandes, herramientas como `gdisk`, `parted` o `blkid` pueden ofrecer características adicionales y mayor seguridad. Siempre comprueba las capacidades de tu versión específica de fdisk con `fdisk --version`.