# Comandos Essenciales de Shell en Linux

Este documento sirve como una referencia rápida y organizada de los comandos más utilizados en sistemas Linux, categorizados por funcionalidad. Cada comando listed tiene un enlace directo a su documentación detallada en el directorio `Comandos/` para facilitar el aprendizaje profundo.

## 📁 Gestión de Archivos y Directorios

Comandos para crear, modificar, navegar y gestionar archivos y sistemas de archivos. [[file-commands-cheat-sheet]]

| Comando | Descripción | Documento |
|---------|-------------|-----------|
| `tar` | Archiva y comprime múltiples archivos | [Ver documento](Comandos/tar.md) |
| `pv` | Monitoriza el progreso de datos através de tuberías | [Ver documento](Comandos/pv.md) |
| `cat` | Concatena y muestra el contenido de archivos | [Comandos/cat.md](cat.md) |
| `tac` | Muestra archivos en orden inverso | [Comandos/tac.md](tac.md) |
| `chmod` | Cambia permisos de archivos y directorios | [Comandos/chmod.md](Comandos/chmod.md) |
| `grep` | Busca patrones de texto dentro de archivos | [Comandos/grep.md](grep.md) |
| `diff` | Compara diferencias entre archivos | [Comandos/diff.md](Comandos/diff.md) |
| `sed` | Editor de flujos para transformar texto | [Comandos/sed.md](Comandos/sed.md) |
| `ar` | Crea, modifica y extrae de archivos estáticos | [Comandos/ar.md](Comandos/ar.md) |
| `man` | Muestra el manual de comandos y funciones | [Comandos/man.md](man.md) |
| `pushd` | Guarda y cambia al directorio especificado | [Comandos/pushd.md](Comandos/pushd.md) |
| `popd` | Restaura el directorio guardado previamente | [Comandos/popd.md](Comandos/popd.md) |
| `seq` | Genera secuencias de números | [Comandos/seq.md](Comandos/seq.md) |
| `fd` | Alternativa simple, rápida y amigable a `find` | [Comandos/fd.md](Comandos/fd.md) |
| `pandoc` | Conversor universal de documentos | [Comandos/pandoc.md](Comandos/pandoc.md) |
| `cd` | Cambia de directorio | [Comandos/cd.md](Comandos/cd.md) |
| `$PATH` | Variable de entorno que define dónde buscar ejecutables | [Comandos/path.md](Comandos/path.md) |
| `awk` | Lenguaje de procesamiento de patrones y campos | [Comandos/awk.md](awk.md) |
| `join` | Une líneas de dos archivos basado en un campo común | [Comandos/join.md](Comandos/join.md) |
| `jq` | Procesador de JSON para línea de comandos | [Comandos/jq.md](Comandos/jq.md) |
| `fold` | Ajusta líneas para que quepan en un ancho especificado | [Comandos/fold.md](Comandos/fold.md) |
| `uniq` | Reporte o omite líneas repetidas | [Comandos/uniq.md](Comandos/uniq.md) |
| `journalctl` | Consulta y muestra los registros del systemd | [Comandos/journalctl.md](Comandos/journalctl.md) |
| `tail` | Muestra las últimas líneas de un archivo | [Comandos/tail.md](Comandos/tail.md) |
| `stat` | Muestra información detallada de archivos y sistemas de archivos | [Comandos/stat.md](Comandos/stat.md) |
| `ls` | Lista el contenido de directorios | [Comandos/ls.md](ls.md) |
| `fstab` | Archivo de configuración de sistemas de archivos montados | [Comandos/fstab.md](Comandos/fstab.md) |
| `echo` | Muestra texto o variables en pantalla | [Comandos/echo.md](echo.md) |
| `less` | Visor de archivos paginado (ideal para archivos grandes) | [Comandos/less.md](Comandos/less.md) |
| `chgrp` | Cambia el grupo propietario de archivos | [Comandos/chgrp.md](Comandos/chgrp.md) |
| `chown` | Cambia el propietario y grupo de archivos | [Comandos/chown.md](Comandos/chown.md) |
| `rev` | Invierte las líneas de un archivo caracter por caracter | [Comandos/rev.md](Comandos/rev.md) |
| `look` | Muestra líneas que comienzan con una cadena específica | [Comandos/look.md](Comandos/look.md) |
| `strings` | Extrae secuencias de caracteres imprimibles de archivos binarios | [Comandos/strings.md](Comandos/strings.md) |
| `type` | Indica cómo sería interpretado un nombre si se usara como comando | [Comandos/type.md](Comandos/type.md) |
| `rename` | Renombra múltiples archivos usando expresiones regulares | [Comandos/rename.md](Comandos/rename.md) |
| `zip` | Crea archivos ZIP (formato multi-plataforma) | [Comandos/zip.md](Comandos/zip.md) |
| `unzip` | Extrae archivos ZIP | [Comandos/unzip.md](Comandos/unzip.md) |
| `install` | Copia archivos y establece sus permisos | [Comandos/install.md](Comandos/install.md) |
| `rm` | Elimina archivos y directorios | [Comandos/rm.md](Comandos/rm.md) |
| `rmdir` | Elimina directorios vacíos | [Comandos/rmdir.md](Comandos/rmdir.md) |
| `rsync` | Sincroniza archivos y directorios entre sistemas | [Comandos/rsync.md](Comandos/rsync.md) |
| `df` | Muestra el uso de espacio en sistemas de archivos montados | [Comandos/df.md](Comandos/df.md) |
| `gpg` | Herramienta de cifrado y firma OpenPGP | [Comandos/gpg.md](Comandos/gpg.md) |
| `vi` | Editor de texto modal potente | [Comandos/vi.md](Comandos/vi.md) |
| `nano` | Editor de texto sencillo y amigable | [Comandos/nano.md](Comandos/nano.md) |
| `mkdir` | Crea directorios | [Comandos/mkdir.md](Comandos/mkdir.md) |
| `du` | Estima el uso de espacio en archivos y directorios | [Comandos/du.md](Comandos/du.md) |
| `ln` | Crea enlaces (duros y simbólicos) entre archivos | [Comandos/ln.md](Comandos/ln.md) |
| `patch` | Aplica parches a archivos originales | [Comandos/patch.md](Comandos/patch.md) |
| `convert` | Parte de ImageMagick para convertir, editar y compilar imágenes | [Comandos/convert.md](Comandos/convert.md) |
| `rclone` | Sincroniza archivos con servicios de almacenamiento en la nube | [Comandos/rclone.md](Comandos/rclone.md) |
| `shred` | Sobrescribe archivos para ocultar su contenido | [Comandos/shred.md](Comandos/shred.md) |
| `srm` | Elimina archivos de manera segura (secure remove) | [Comandos/srm.md](Comandos/srm.md) |

## ⚙️ Gestión de Procesos y Servicios

Comandos para monitorear, controlar y administrar procesos en ejecución. [[gestion-de-procesos-en-linux]]

| Comando | Descripción | Documento |
|---------|-------------|-----------|
| `alias` | Crea alias (nombres alternativos) para comandos | [Comandos/alias.md](Comandos/alias.md) |
| `screen` | Administrador de terminales multiplexadas | [Comandos/screen.md](Comandos/screen.md) |
| `nice` | Ejecuta un comando con prioridad ajustada | [Comandos/nice.md](Comandos/nice.md) |
| `renice` | Cambia la prioridad de un proceso en ejecución | [Comandos/renice.md](Comandos/renice.md) |
| `progress` | Muestra barras de progreso para comandos en tubería | [Comandos/progress.md](Comandos/progress.md) |
| `strace` | Rastrea llamadas al sistema y señales | [Comandos/strace.md](Comandos/strace.md) |
| `systemd` | Sistema de init y administración de servicios | [Comandos/systemd.md](Comandos/systemd.md) |
| `tmux` | Administrador de terminales multiplexadas moderno | [Comandos/tmux.md](Comandos/tmux.md) |
| `chsh` | Cambia el shell de inicio de sesión de un usuario | [Comandos/chsh.md](Comandos/chsh.md) |
| `history` | Muestra y gestiona el historial de comandos | [Comandos/history.md](history.md) |
| `at` | Programa comandos para ejecución única en el futuro | [Comandos/at.md](Comandos/at.md) |
| `batch` | Programa comandos para ejecución cuando el sistema esté menos cargado | [Comandos/batch.md](Comandos/batch.md) |
| `which` | Localiza el ejecutable asociado a un comando | [Comandos/which.md](Comandos/which.md) |
| `dmesg` | Muestra el mensaje del anillo de control del kernel | [Comandos/dmesg.md](Comandos/dmesg.md) |
| `chfn` | Cambia la información completa del finger de un usuario | [Comandos/chfn.md](Comandos/chfn.md) |
| `usermod` | Modifica una cuenta de usuario existente | [Comandos/usermod.md](Comandos/usermod.md) |
| `ps` | Muestra una instantánea de los procesos activos | [Comandos/ps.md](ps.md) |
| `xargs` | Construye y ejecuta comandos desde entrada estándar | [Comandos/xargs.md](Comandos/xargs.md) |
| `tty` | Muestra el nombre del terminal conectado a la entrada estándar | [Comandos/tty.md](Comandos/tty.md) |
| `pinky` | Versión mejorada y ligera del comando finger | [Comandos/pinky.md](Comandos/pinky.md) |
| `lsof` | Lista archivos abiertos por procesos | [Comandos/lsof.md](Comandos/lsof.md) |
| `vmstat` | Reporta estadísticas de memoria virtual | [Comandos/vmstat.md](Comandos/vmstat.md) |
| `timeout` | Ejecuta un comando con límite de tiempo | [Comandos/timeout.md](Comandos/timeout.md) |
| `wall` | Envía un mensaje a todos los usuarios con sesiones activos | [Comandos/wall.md](Comandos/wall.md) |
| `yes` | Imprime repetidamente una cadena de texto | [Comandos/yes.md](Comandos/yes.md) |
| `kill` | Envía una señal a un proceso | [Comandos/kill.md](Comandos/kill.md) |
| `sleep` | Retarda por un tiempo especificado | [Comandos/sleep.md](Comandos/sleep.md) |
| `time` | Mide el tiempo que tarda un comando en ejecutarse | [Comandos/time.md](Comandos/time.md) |
| `crontab` | Programa comandos para ejecución periódica | [Comandos/crontab.md](Comandos/crontab.md) |
| `date` | Muestra o establece la fecha y hora del sistema | [Comandos/date.md](date.md) |
| `bg` | Continúa un trabajo detenido en segundo plano | [Comandos/bg.md](Comandos/bg.md) |
| `fg` | Trae un trabajo detenido o en segundo plano al foreground | [Comandos/fg.md](Comandos/fg.md) |

## 🌐 Comunicación y Redes

Comandos para configurar, diagnosticar y solucionar problemas de conectividad de red. [[comandos-de-red-en-linux]]

| Comando | Descripción | Documento |
|---------|-------------|-----------|
| `netstat` | Muestra conexiones, tablas de enrutamiento, estadísticas de interfaces | [Comandos/netstat.md](Comandos/netstat.md) |
| `ping` | Prueba de conectividad usando ICMP Echo | [Comandos/ping.md](Comandos/ping.md) |
| `traceroute` | Muestra el camino que siguen los paquetes a un destino | [Comandos/traceroute.md](Comandos/traceroute.md) |
| `ip` | Herramienta moderna para gestionar interfaces, rutas y más | [Comandos/ip.md](Comandos/ip.md) |
| `ss` | Sustituto moderno de netstat, más rápido y con más información | [Comandos/ss.md](Comandos/ss.md) |
| `whois` | Consulta registros de dominios y direcciones IP | [Comandos/whois.md](Comandos/whois.md) |
| `bmon` | Monitor de ancho de banda y tasa de error con soporte para múltiples interfaces | [Comandos/bmon.md](Comandos/bmon.md) |
| `dig` | Herramienta flexible para consultar servidores DNS | [Comandos/dig.md](Comandos/dig.md) |
| `finger` | Obtiene información sobre usuarios del sistema | [Comandos/finger.md](Comandos/finger.md) |
| `nmap` | Escáner de puertos y servicios de red | [Comandos/nmap.md](Comandos/nmap.md) |
| `ftp` | Cliente de Protocolo de Transferencia de Archivos | [Comandos/ftp.md](Comandos/ftp.md) |
| `curl` | Transfiere datos desde o hacia un servidor | [Comandos/curl.md](Comandos/curl.md) |
| `wget` | Descarga archivos desde la web | [Comandos/wget.md](Comandos/wget.md) |
| `who` | Muestra quién está conectado al sistema | [Comandos/who.md](Comandos/who.md) |
| `whoami` | Muestra el nombre de usuario efectivo actual | [Comandos/whoami.md](Comandos/whoami.md) |
| `w` | Muestra quién está conectado y qué están haciendo | [Comandos/w.md](Comandos/w.md) |
| `iptables` | Configura tablas de filtrado y NAT de IPv4 | [Comandos/iptables.md](iptables.md) |
| `ssh-keygen` | Genera, gestiona y convierte claves de autenticación SSH | [Comandos/ssh-keygen.md](Comandos/ssh-keygen.md) |
| `ufw` | Interfaz sencilla para configurar firewall (Uncomplicated Firewall) | [Comandos/ufw.md](Comandos/ufw.md) |

## 💾 Hardware y Sistema

Comandos para obtener información y gestionar componentes de hardware.

| Comando | Descripción | Documento |
|---------|-------------|-----------|
| `lscpu` | Muestra información de la arquitectura de la CPU | [Comandos/lscpu.md](Comandos/lscpu.md) |
| `lshw` | Lista detallada de la configuración de hardware | [Comandos/lshw.md](Comandos/lshw.md) |
| `lspci` | Lista dispositivos PCI y sus controladores | [Comandos/lspci.md](Comandos/lspci.md) |
| `lsusb` | Lista dispositivos USB y sus controladores | [Comandos/lsusb.md](Comandos/lsusb.md) |
| `free` | Muestra la cantidad de memoria libre y usada | [Comandos/free.md](Comandos/free.md) |
| `top` | Muestra procesos en tiempo real y uso de recursos | [Comandos/top.md](Comandos/top.md) |
| `du` | Estima el uso de espacio en archivos y directorios | [Comandos/du.md](Comandos/du.md) |
| `shutdown` | Apaga o reinicia el sistema | [Comandos/shutdown.md](Comandos/shutdown.md) |
| `reboot` | Reinicia el sistema | [Comandos/reboot.md](Comandos/reboot.md) |
| `halt` | Detiene el funcionamiento de la CPU | [Comandos/halt.md](Comandos/halt.md) |
| `poweroff` | Apaga el sistema mediante corte de energía | [Comandos/poweroff.md](Comandos/poweroff.md) |

## 💾 Sistema de Archivos

Comandos específicos para gestionar sistemas de almacenamiento y particiones.

| Comando | Descripción | Documento |
|---------|-------------|-----------|
| `mount` | Monta sistemas de archivos en puntos de montaje | [Comandos/mount.md](Comandos/mount.md) |
| `umount` | Desmonta sistemas de archivos | [Comandos/umount.md](Comandos/umount.md) |
| `fdisk` | Manipula tablas de particiones de discos | [Comandos/fdisk.md](fdisk.md) |
| `mkfs` | Crea sistemas de archivos en particiones | [Comandos/mkfs.md](Comandos/mkfs.md) |
| `fsck` | Verifica y repara sistemas de archivos | [Comandos/fsck.md](Comandos/fsck.md) |
| `testdisk` | Recupera particiones perdidas y hace que discos no booteables vuelvan a bootear | [Comandos/testdisk.md](Comandos/testdisk.md) |
| `file` | Determina el tipo de archivo basado en su contenido | [Comandos/file.md](file.md) |

## 👥 Usuarios y Grupos

Comandos para gestionar cuentas de usuario y grupos en el sistema.

| Comando | Descripción | Documento |
|---------|-------------|-----------|
| `groupadd` | Crea un nuevo grupo | [Comandos/groupadd.md](Comandos/groupadd.md) |
| `usermod` | Modifica una cuenta de usuario existente | [Comandos/usermod.md](Comandos/usermod.md) |
| `groups` | Muestra las pertenencias a grupos de un usuario | [Comandos/groups.md](Comandos/groups.md) |
| `passwd` | Modifica la contraseña de un usuario | [Comandos/passwd.md](Comandos/passwd.md) |
| `sudo` | Ejecuta comandos con privilegios de seguridad de otro usuario | [Comandos/sudo.md](Comandos/sudo.md) |
| `su` | Cambia a otra identidad de usuario durante una sesión de sesión | [Comandos/su.md](Comandos/su.md) |
| `chroot` | Cambia el directorio raíz para un proceso y sus hijos | [Comandos/chroot.md](Comandos/chroot.md) |
| `chgrp` | Cambia el grupo propietario de archivos | [Comandos/chgrp.md](Comandos/chgrp.md) |
| `chown` | Cambia el propietario y grupo de archivos | [Comandos/chown.md](Comandos/chown.md) |
| `pwd` | Muestra el nombre del directorio de trabajo actual | [Comandos/pwd.md](pwd.md) |

## 🐧 Shell y Entorno

Comandos específicos del entorno de shell y variables de entorno.

| Comando | Descripción | Documento |
|---------|-------------|-----------|
| `echo` | Muestra texto o variables en pantalla | [Comandos/echo.md](echo.md) |
| `yes` | Imprime repetidamente una cadena de texto | [Comandos/yes.md](Comandos/yes.md) |

## 🗃️ Base de Datos

| Comando | Descripción | Documento |
|---------|-------------|-----------|
| `sqlite3` | Interfaz de línea de comandos para SQLite version 3 | [Comandos/sqlite.md](sqlite.md) |

## 📚 Cómo usar este documento

1. **Navega por categoría**: Encuentra el tipo de comando que necesitas según su funcionalidad
2. **Sigue los enlaces**: Cada comando tiene un enlace directo a su documentación detallada
3. **Explora en profundidad**: Los documentos individuales proporcionan ejemplos prácticos, opciones avanzadas y mejores prácticas
4. **Combina comandos**: Muchos de estos herramientas están diseñados para trabajar juntos mediante tuberías (`|`) y redirecciones

> **Nota**: Este documento es una referencia curada de los comandos más comunes y útiles. Para una lista completa de todos los comandos disponibles en tu sistema, puedes explorar los directorios `/bin`, `/usr/bin`, `/usr/local/bin`, etc., o usar `compgen -c` en bash para listar todos los comandos disponibles.