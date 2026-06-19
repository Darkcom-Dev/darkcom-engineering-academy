# Ps: Monitorización de Procesos en Linux

El comando `ps` (process status) es una herramienta fundamental en sistemas Linux para monitorear y gestionar los procesos en ejecución. Proporciona una instantánea detallada de los procesos activos en el sistema, incluyendo información como identificadores de proceso (PID), uso de recursos, estado y mucho más. Es esencial para administradores de sistemas, desarrolladores y cualquier usuario que necesite diagnosticar problemas de rendimiento o gestionar aplicaciones.

## ¿Para qué se utiliza?

- **Visualizar procesos activos** en el sistema en un momento dado
- **Identificar consumo de recursos** (CPU, memoria) por proceso
- **Diagnosticar problemas** como procesos colgados o que consumen excesivos recursos
- **Gestionar procesos** mediante su combinación con comandos como `kill` o `renice`
- **Verificar estado** de servicios y aplicaciones críticas
- **Auditar actividad** del sistema para seguridad y rendimiento

## Sintaxis Básica

```bash
ps [opciones]
```

A diferencia de muchos comandos de Linux, `ps` acepta opciones en tres estilos diferentes:
- **Estilo UNIX**: opciones que comienzan con `-` y pueden agruparse (ej: `-aux`)
- **Estilo BSD**: opciones que no usan guión (ej: `aux`)
- **Estilo GNU largo**: opciones que comienzan con `--` (ej: `--sort=%cpu`)

## Campos de Salida Más Comunes

`ps` puede mostrar una amplia variedad de campos. Aquí están los más útiles:

| Campo | Descripción | Ejemplo de Valor |
|-------|-------------|------------------|
| `PID` | Identificador de proceso único | `1234` |
| `PPID` | ID del proceso padre | `1` |
| `USER` o `UID` | Usuario propietario del proceso | `www-data` |
| `%CPU` | Porcentaje de CPU utilizada | `2.5` |
| `%MEM` | Porcentaje de memoria RAM utilizada | `15.2` |
| `VSZ` | Tamaño de memoria virtual en KiB | `125828` |
| `RSS` | Resident Set Size (memoria física real) en KiB | `45216` |
| `TTY` | Terminal asociada al proceso (`?` para ninguno) | `pts/0` |
| `STAT` | Estado del proceso (ver tabla abajo) | `Ss` |
| `START` | Hora de inicio del proceso | `08:30` |
| `TIME` | Tiempo total de CPU consumido | `00:01:23` |
| `COMMAND` o `CMD` | Comando que inició el proceso | `/usr/sbin/apache2` |

### Estados del Proceso (STAT)
El campo `STAT` muestra el estado actual del proceso usando códigos:

| Código | Descripción |
|--------|-------------|
| `D` | Sueño ininterrumpible (usually I/O) |
| `I` | Sueño inactivo (kernel thread) |
| `R` | En ejecución o ejecutable (runnable) |
| `S` | Sueño interrumpible (esperando evento) |
| `T` | Detenido por señal de control de trabajo |
| `t` | Detenido por depurador durante tracing |
| `W` | Paginando (no válido desde kernel 2.6) |
| `X` | Estado muerto (should never be seen) |
| `Z` | Proceso zombie (terminado pero no recogido por padre) |
| `<` | Prioridad alta (no nice al usuario) |
| `N` | Prioridad baja (nice al usuario) |
| `L` | Tiene páginas bloqueadas en memoria (para IO en tiempo real) |
| `s` | Es líder de sesión |
| `l` | Es líder de grupo de procesos (CPL) |
| `+` | Está en el grupo de procesos foreground |

## Opciones Más Utilizadas

### Formatos Predefinidos (Estilos Tradicionales)
Estos son los formatos más comunes que proporcionan una buena visión general:

| Opción | Descripción | Cuándo Usarla |
|--------|-------------|---------------|
| `ps aux` | **Formato BSD**: muestra todos los procesos de todos los usuarios con formato detallado | Uso cotidiano, diagnóstico general |
| `ps -ef` | **Formato completo**: muestra todos los procesos con formato estándar | Ver relaciones padre/hijo claramente |
| `ps -eo` **formato personalizado** | Permite especificar exactamente qué campos mostrar | Cuando necesitas información específica |
| `ps -L` | Muestra información de hilos (threads) además de procesos | Diagnóstico de aplicaciones multihilo |

### Opciones de Selección de Procesos
Estas opciones determinan qué procesos se muestran:

| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-A`, `-e` | Selecciona todos los procesos | `ps -e` |
| `-a` | Selecciona todos excepto líderes de sesión y procesos sin tty | `ps -a` |
| `-r` | Restringe a procesos en ejecución | `ps -r` |
| `-T` | Selecciona todos los procesos en esta terminal | `ps -T` |
| `-u` **usuario** | Selecciona procesos por usuario efectivo | `ps -u www-data` |
| `-U` **usuario** | Selecciona procesos por usuario real | `ps -U oracle` |
| `-p` **pid** | Selecciona procesos por ID de proceso | `ps -p 1,1234,5678` |
| `-C` **comando** | Selecciona por nombre de comando ejecutable | `ps -C apache2` |
| `-t` **tty** | Selecciona por terminal asociada | `ps -t pts/0` |
| `-g` **pgrp** | Selecciona por ID de grupo de proceso | `ps -g 123` |
| `-G` **pgid** | Selecciona por ID real de grupo de proceso | `ps -G 456` |
| `--sid` **session** | Selecciona por ID de sesión | `ps --sid 789` |

### Opciones de Formato y Ordenamiento
Estas opciones controlan cómo se presenta la información:

| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-o` **formato** | Define formato personalizado de salida | `ps -eo pid,tt,user,%cpu,%mem,comm` |
| `--sort` **especificador** | Ordena la salida por campo especificado | `ps -eo pid,%cpu --sort=-%cpu` (descendente) |
| `--headers` | Repite encabezados en cada página (útil con `less`) | `ps aux --headers \| less -S` |
| `--no-headers` | Suprime la línea de encabezado | `ps aux --no-headers \| wc -l` |
| `-w`, `-ww` | Usa ancho ilimitado para salida (evita truncamiento) | `ps -eww` |
| `-j` | Formato de trabajo (job control) | `ps -j` |
| `-l` | Formato largo | `ps -l` |

## Ejemplos Prácticos

### Visualización General del Sistema
```bash
# Vista completa de todos los procesos (formato BSD)
ps aux

# Vista completa con formato estándar UNIX
ps -ef

# Solo procesos del usuario actual
ps -u $(whoami) -o pid,%cpu,%mem,comm
```

### Identificación de Problemas de Rendimiento
```bash
# Top 10 procesos por consumo de CPU
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -11

# Top 10 procesos por consumo de memoria
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head -11

# Procesos que han estado ejecutándose por más de 1 hora
ps -eo pid,etimes,pid,user,cmd --sort=etimes | awk '$2>3600'

# Procesos en estado interrumpible (posiblemente esperando I/O)
ps -eo pid,stat,cmd | grep '^[[:space:]]*[0-9]\+[[:space:]]*S'
```

### Gestión y Diagnóstico de Servicios
```bash
# Ver todos los procesos relacionados con Apache
ps -ef | grep apache
# o más precisamente:
ps -C apache2 -o pid,ppid,user,%cpu,%mem,vsz,rss,stat,start,time,cmd

# Ver procesos de base de datos MySQL
ps -U mysql -o pid,%cpu,%mem,vsz,rss,stat,start,time,cmd

# Ver procesos Java con argumentos completos
ps -C java -o pid,%cpu,%mem,args

# Ver todos los procesos de un servicio específico (ej: nginx)
ps -C nginx -o pid,ppid,pgid,user,%cpu,%mem,stat,start,time,cmd
```

### Análisis de Hilos y Subprocesos
```bash
# Ver hilos de un proceso específico (reemplaza 1234 con el PID)
ps -L -p 1234 -o tid,pcpu,pmem,comm

# Ver todos los hilos en el sistema ordenados por consumo de CPU
ps -eLf --sort=-pcpu

# Contar hilos por proceso
ps -eLf | awk '{print $2}' | sort | uniq -c | sort -nr
```

### Uso en Scripts y Automatización
```bash
# Verificar si un proceso específico está ejecutándose
if pgrep -x "nginx" > /dev/null; then
    echo "Nginx está ejecutándose"
else
    echo "Nginx NO está ejecutándose"
fi

# Obtener el PID de un proceso por nombre
PID=$(pgrep -f "java.*-jar miapp.jar")
if [ -n "$PID" ]; then
    echo "PID de miapp.jar: $PID"
    ps -p $PID -o %cpu,%mem,vsz,rss,stat
fi

# Matar procesos que consumen más del 80% de CPU
ps -eo pid,%cpu --sort=-%cpu | awk '$2>80.0 {print $1}' | xargs -r kill -9

# Reiniciar un servicio si su uso de memoria supera un límite
MEM_USAGE=$(ps -C apache2 -o %mem= | awk '{sum+=$1} END {print sum}')
if (( $(echo "$MEM_USAGE > 50.0" | bc -l) )); then
    echo "Reiniciando Apache por alto uso de memoria: $MEM_USAGE%"
    systemctl restart apache2
fi
```

### Formatos de Salida Personalizados Útiles
```bash
# Formato compacto para monitoreo continuo
ps -eo pid,user,%cpu,%mem,vsz,rss,tt,stat,start,time,cmd --sort=-%cpu

# Formato para análisis forense (incluye información de seguridad)
ps -eo pid,user,uid,group,pid,%cpu,%mem,vsz,rss,stat,start,time,args

# Formato mínimo para procesamiento con otras herramientas
ps -eo pid,ppid,%cpu,%mem,comm --no-headers

# Mostrar tiempo de ejecución formateado (días-hh:mm:ss)
ps -eo pid,user,etime,pcpu,pmem,comm --sort=-etime

# Mostrar proceso con su entorno (útil para depuración)
ps -eo pid,user,comm --sort=pid | while read pid user cmd; do
    echo "=== PID $pid ($user) $cmd ==="
    tr '\0' '\n' < /proc/$pid/environ | head -5
    echo
done
```

## Buenas Prácticas y Consejos

### Para un Uso Efectivo
1. **Usa `ps aux` como punto de partida** para obtener una visión general rápida del sistema
2. **Combina con `grep` con cuidado** para evitar coincidencias falsas:
   ```bash
   # En lugar de esto (que puede coincidir con el propio grep):
   ps aux | grep apache
   
   # Usa esto para evitar el falso positivo:
   ps aux | grep [a]pache
   
   # O mejor aún, usa pgrep:
   pgrep -l apache
   ```
3. **Especifica exactamente qué campos necesitas** en lugar de confiar en los formatos predeterminados cuando escribes scripts
4. **Ten cuidado con los porcentajes de CPU** en sistemas con múltiples núcleos - el porcentaje puede exceder 100% en sistemas SMP
5. **Usa opciones de ancho ilimitado** (`-ww`) cuando necesites ver comandos completos sin truncamiento

### Para Solución de Problemas de Rendimiento
1. **Monitorea tendencias**, no solo instantáneas: ejecuta `ps` periódicamente y registra los resultados
2. **Presta atención a los procesos en estado `D`** (uninterruptible sleep) - pueden indicar problemas de I/O o kernel
3. **Revisa los procesos zombie (`Z`)** - aunque generalmente inofensivos en pequeñas cantidades, muchos pueden indicar problemas con aplicaciones padre
4. **Verifica la relación entre VSZ y RSS** - una gran diferencia puede indicar uso intensivo de swap
5. **Observa procesos con alto `%CPU` sostenido** - pueden indicar bucles infinitos o tareas computacionalmente costosas

### Para Scripts de Monitoreo
1. **Usa formato personalizado** (`-o`) para obtener solo los datos que necesitas
2. **Suprime encabezados** (`--no-headers`) cuando procesas la salida con otras herramientas
3. **Considera el uso de `ps` en combinación con herramientas de monitoreo** como `top`, `htop` o `glances` para vista en tiempo real
4. **Recuerda que `ps` muestra una instantánea** - para monitoreo continuo, considera usar herramientas diseñadas específicamente para eso
5. **En sistemas de producción**, registra la salida de `ps` en logs para análisis forense posterior

### Limitaciones y Alternativas
Aunque `ps` es poderoso, tiene algunas limitaciones:
- Muestra solo una instantánea, no tendencias en tiempo real
- Puede ser costoso en sistemas con miles de procesos
- Algunos campos requieren privilegios especiales para acceder
- La interpretación de algunos estados puede ser compleja

**Alternativas según el caso de uso**:
- Para monitoreo en tiempo real: `top`, `htop`, `glances`
- Para análisis histórico: herramientas de monitoreo como Prometheus, Nagios, Zabbix
- Para información detallada de un proceso específico: examinar `/proc/[pid]/`
- Para rastreo de llamadas al sistema: `strace`
- Para análisis de rendimiento: `perf`, `sysstat`

## Integración con Otras Herramientas

### Con `kill` y Gestión de Señales
```bash
# Enviar señal TERM a todos los procesos de un usuario
pkill -u usuario_especifico

# Enviar señal KILL a procesos que coinciden con un patrón
pkill -f "patrón peligroso"

# Renice (ajustar prioridad) de un proceso
renice -n 10 -p 1234  # Aumenta niceness (reduce prioridad)
renice -n -5 -p 5678  # Reduce niceness (aumenta prioridad - requiere root)
```

### Con Herramientas de Análisis de `/proc`
```bash
# Obtener información detallada de un proceso desde /proc
PID=1234
echo "=== Información de /proc/$PID ==="
echo "Nombre del ejecutable: $(cat /proc/$PID/comm)"
echo "Estado: $(cat /proc/$PID/status | grep State)"
echo "Memoria usada: $(cat /proc/$PID/status | grep VmRSS) kB"
echo "Límites de recursos: $(cat /proc/$PID/limits)"
echo "Directorios de trabajo: $(ls -la /proc/$PID/cwd)"
```

### Con Herramientas de Red
```bash
# Ver qué procesos están escuchando en puertos de red
sudo netstat -tulpn | grep LISTEN
# o con ss (más moderno)
sudo ss -tulpn

# Ver conexiones de red por proceso
sudo lsof -i -P -n | grep LISTEN

# Combinar con ps para obtener más detalles
sudo lsof -i -P -n | grep LISTEN | awk '{print $2}' | xargs -I {} ps -p {} -o pid,user,comm
```

## Recursos Adicionales

- Para ver el manual completo: `man ps`
- Para información detallada sobre formato de salida: `man ps` (busca la sección "STANDARD FORMAT SPECIFIERS")
- Para ejemplos avanzados: consulta la documentación de `proc` (`man 5 proc`)
- Para alternativas modernas: explora `htop` (interfaz interactiva mejorada) o `atop` (monitoreo de recursos histórico)
- Para monitoreo a nivel de sistema: considera herramientas como `collectl`, `dstat` o `glances`

> **Nota**: Dominar el comando `ps` es esencial para cualquier trabajo serio en entornos Linux. Su capacidad para proporcionar información detallada sobre el estado del sistema lo convierte en la primera herramienta de diagnóstico cuando se enfrentan a problemas de rendimiento, comportamiento inesperado de aplicaciones o necesidades de gestión de procesos. La práctica constante con las técnicas descritas aquí le permitirá obtener rápidamente la información necesaria para mantener sistemas Linux sanos y eficientes.