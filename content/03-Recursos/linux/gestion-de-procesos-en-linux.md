# Gestión de Procesos en Linux

Los procesos son programas en ejecución dentro del sistema operativo Linux. Gestionarlos eficazmente es esencial para la administración del sistema, diagnóstico de problemas y optimización de recursos. Esta guía cubre los comandos más importantes para monitorear, controlar y administrar procesos.

## ¿Qué es un proceso?

Un proceso es una instancia de un programa en ejecución. Cada proceso tiene:
- **PID (Process ID)**: Identificador único numérico
- **PPID (Parent Process ID)**: ID del proceso padre que lo creó
- **Estado**: Ejecutando, dormido, detenido, zombie, etc.
- **Recursos**: Uso de CPU, memoria, archivos abiertos, etc.
- **Usuario y grupo**: Propietario del proceso y sus permisos

## Comandos de Visualización de Procesos

### ps (Process Status)
Muestra una instantánea de los procesos activos.

**Sintaxis básica**: `ps [opciones]`

**Opciones más útiles**:
| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `aux` | Formato BSD: todos los procesos de todos los usuarios con formato detallado | `ps aux` |
| `-ef` | Formato completo estándar: muestra todos los procesos con formato completo | `ps -ef` |
| `-eo formato` | Define formato personalizado de salida | `ps -eo pid,ppid,user,%cpu,%mem,comm` |
| `-L` | Muestra información de hilos (threads) además de procesos | `ps -L` |
| `-t tty` | Filtra por terminal asociada | `ps -t pts/0` |
| `-u usuario` | Filtra por usuario efectivo | `ps -u www-data` |
| `-C comando` | Filtra por nombre exacto de comando | `ps -C apache2` |
| `--sort=campo` | Ordena la salida por campo especificado (usar `-` para descendente) | `ps aux --sort=-%cpu` |

**Ejemplos prácticos**:
```bash
# Ver todos los procesos con formato detallado
ps aux

# Ver procesos ordenados por consumo de CPU (mayor a menor)
ps aux --sort=-%cpu

# Ver solo procesos de Apache
ps -C apache2 -o pid,ppid,user,%cpu,%mem,vsz,rss,stat,start,time,cmd

# Ver hilos de un proceso específico (reemplaza 1234 con el PID)
ps -L -p 1234 -o tid,pcpu,pmem,comm

# Formato personalizado para monitoreo
ps -eo pid,user,%cpu,%mem,vsz,rss,tt,stat,start,time,cmd --sort=-%cpu
```

### top
Monitor de procesos en tiempo real con actualización continua.

**Uso básico**: `top`

**Teclas interactivas útiles**:
- `Shift+P`: Ordenar por %CPU (predeterminado)
- `Shift+M`: Ordenar por %MEM
- `Shift+T`: Ordenar por tiempo CPU
- `k`: Enviar señal a un proceso (necesita PID)
- `r`: Renice (cambiar prioridad) de un proceso
- `f`: Administrar campos mostrados
- `q`: Salir

**Variantes mejoradas**:
- **htop**: Interfaz más amigable con colores y soporte para mouse
- **atop**: Monitoreo de recursos con registro histórico
- **glances**: Multiplataforma con interfaz web opcional

### jobs
Muestra trabajos gestionados por el shell actual (en segundo plano, detenidos, etc.).

**Ejemplo**:
```bash
# Iniciar un proceso en segundo plano
sleep 300 &

# Ver trabajos activos
jobs
# [1]+  Running                 sleep 300 &

# Traer trabajo al foreground
fg %1

# Detener trabajo
kill %1
```

## Envío de Señales a Procesos

Los procesos pueden recibir señales que les indican realizar ciertas acciones (terminar, pausar, recargar configuración, etc.).

### kill
Envía una señal a uno o más procesos especificados por PID.

**Sintaxis**: `kill [señal] PID...`

**Señales más comunes**:
| Señal | Valor | Descripción |
|-------|-------|-------------|
| `SIGTERM` | 15 | Terminación solicitada (predeterminada si no se especifica) |
| `SIGKILL` | 9 | Terminación forzada (no puede ser ignorada) |
| `SIGHUP` | 1 | Vuelva a leer configuración (usado por muchos daemons) |
| `SIGINT` | 2 | Interrupción desde teclado (Ctrl+C) |
| `SIGSTOP` | 19 | Detener proceso (puede ser continuado con SIGCONT) |
| `SIGCONT` | 18 | Continuar proceso detenido |

**Ejemplos**:
```bash
# Enviar SIGTERM (predeterminado) al PID 1234
kill 1234

# Forzar terminación con SIGKILL
kill -9 1234

# Hacer que un daemon vuelva a leer su configuración
kill -HUP $(cat /var/run/sshd.pid)

# Detener un proceso temporalmente
kill -STOP 5678
# Más tarde continuarlo
kill -CONT 5678
```

### pkill y killall
Envían señales basadas en nombre de proceso u otros atributos.

**pkill**: Envía señal basado en nombre u otros criterios
```bash
# Terminar todos los procesos llamados firefox
pkill firefox

# Terminar procesos de un usuario específico
pkill -u usuario

# Enviar señal basado en coincidencia completa del comando
pkill -f "java -jar miapp.jar"
```

**killall**: Similar a pkill pero con comportamiento ligeramente diferente
```bash
# Terminar todos los procesos llamado gnome-terminal
killall gnome-terminal
```

## Prioridad y Programación de Procesos

### nice y renice
Ajustan la "amabilidad" (nice value) de un proceso, afectando su prioridad de programación.

- Valores de nice van de -20 (máxima prioridad) a 19 (mínima prioridad)
- Solo root puede establecer valores negativos (prioridad alta)
- Valor predeterminado suele ser 0

**Ejemplos**:
```bash
# Iniciar un proceso con baja prioridad (nice 10)
nice -n 10 comando_largo

# Cambiar la prioridad de un proceso existente (reemplaza 1234 con PID)
renice -n 5 -p 1234

# Ver el nice value de un proceso
ps -o pid,ni,comm -p 1234
```

## Otros Comandos Útiles

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| **bg** | Continúa un trabajo detenido en segundo plano | `bg %1` |
| **fg** | Trae un trabajo detenido o en segundo plano al foreground | `fg %1` |
| **nohup** | Ejecuta un comando inmune a señales de desconexión (SIGHUP) | `nohup largo_proceso &` |
| **disown** | Elimina un trabajo de la tabla de trabajos del shell | `disown %1` |
| **time** | Mide el tiempo que tarda un comando en ejecutarse | `time comando` |
| **timeout** | Ejecuta un comando con límite de tiempo | `timeout 30s comando` |
| **watch** | Ejecuta un comando periódicamente y muestra salida en pantalla | `watch -n 5 'ps aux --sort=-%cpu \| head -10'` |
| **strace** | Rastrea llamadas al sistema y señales | `straz -p 1234` |
| **ltrace** | Rastrea llamadas a bibliotecas compartidas | `ltrace -p 1234` |
| **vmstat** | Reporta estadísticas de memoria virtual | `vmstat 1` |
| **iostat** | Reporta estadísticas de entrada/salida de dispositivos | `iostat -xz 1` |
| **lsof** | Lista archivos abiertos por procesos (incluye sockets, archivos, etc.) | `lsof -i -P -n` (conexiones de red) |
| **systemctl** | Controla el init system systemd (servicios) | `systemctl status apache2` |
| **service** | Compatibilidad con scripts init tradicionales | `service apache2 status` |
| **chkconfig** | Gestiona servicios en niveles de ejecución (SysV init) | `chkconfig apache2 on` |

## Buenas Prácticas y Consejos

### Para Monitoreo Efectivo
1. **Use `ps aux` como punto de partida** para una visión general del sistema
2. **Ordene por recurso relevante** según lo que esté investigando (CPU, memoria, I/O)
3. **Combine múltiples herramientas** para diagnóstico completo (ej: `top` + `vmstat` + `iostat`)
4. **Use formato personalizado** en scripts para obtener solo los datos necesarios
5. **Considere herramientas de monitoreo continuo** en entornos de producción (Prometheus, Zabbix, etc.)

### Para Gestión de Procesos
1. **Prefiera SIGTERM sobre SIGKILL** cuando sea posible, permite limpieza adecuada
2. **Verifique el PID antes de matar** procesos críticos
3. **Use `pkill -f` con extremo cuidado** - puede coincidir con más procesos de lo esperado
4. **Documente cambios de prioridad** en sistemas de producción
5. **Use `nohup` o `disown`** para procesos que deben sobrevivir al cierre de terminal
6. **Agrupe procesos relacionados** usando cgroups cuando sea necesario para control de recursos

### Para Solución de Problemas
1. **Busca procesos en estado `D`** (uninterruptible sleep) - pueden indicar problemas de I/O o kernel
2. **Revisa procesos zombie (`Z`)** - aunque inofensivos en pequeñas cantidades, muchos pueden indicar problemas con procesos padre que no hacen wait()
3. **Monitorea procesos con alto `%CPU` sostenido** - pueden indicar bucles infinitos o tareas costosas
4. **Verifica fugas de memoria** observando crecimiento constante de RSS en `ps aux`
5. **Revisa archivos `/proc/[pid]/fd/`** para ver qué archivos tiene abierto un proceso
6. **Use `strace`** para diagnosticar procesos que parecen colgados

### Limitaciones y Consideraciones
- `ps` muestra solo una instantánea, no tendencias históricas
- En sistemas con miles de procesos, algunos comandos pueden ser lentos
- Algunas informaciones requieren privilegios de root (ej: detalles de procesos de otros usuarios)
- La interpretación de algunos estados de proceso puede requerir conocimiento del kernel interno

> **Nota**: Dominar la gestión de procesos es fundamental para cualquier administrador de sistemas o desarrollador que trabaje con Linux. La combinación de las herramientas descritas aquí permite desde diagnóstico básico hasta análisis profundo del comportamiento del sistema y las aplicaciones que en él se ejecutan.