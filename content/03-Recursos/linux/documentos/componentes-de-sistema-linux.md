# Componentes de un sistema Linux

Un sistema Linux funcional está compuesto por varias capas de software que trabajan juntas. A continuación se describen los componentes esenciales y el orden en que intervienen durante el arranque.

## 1. Kernel

Es el núcleo del sistema operativo. Se encarga de:

- Gestionar la memoria (virtual, paginación)
- Planificar y ejecutar procesos (CPU scheduling)
- Comunicarse con el hardware a través de drivers
- Manejar el sistema de archivos (VFS)
- Proveer llamadas al sistema (syscalls)

El kernel se obtiene desde [kernel.org](https://kernel.org). Linux sigue el modelo de kernel monolítico con soporte para módulos cargables (`lsmod`, `modprobe`).

## 2. Bootloader

El kernel en el disco duro no es reconocido por la BIOS/UEFI por sí solo. El bootloader es el encargado de cargarlo en memoria y pasarle los parámetros de arranque.

| Bootloader  | Descripción                                |
| ----------- | ------------------------------------------ |
| GRUB        | El más común en distribuciones Linux       |
| SysLinux    | Usado en sistemas live y USB               |
| ELILO       | Bootloader para sistemas UEFI antiguos     |
| rEFInd      | Interfaz gráfica para UEFI                 |
| systemd-boot| Bootloader minimalista integrado con UEFI  |

## 3. Init (Sistema de inicio)

El kernel arranca, pero necesita un proceso **init** (PID 1) para lanzar el resto del sistema. Init monta sistemas de archivos, inicia servicios y gestiona sesiones.

| Init       | Descripción                                   |
| ---------- | --------------------------------------------- |
| systemd    | Estándar actual en la mayoría de distribuciones |
| OpenRC     | Init basado en scripts, usado en Gentoo       |
| Runit      | Init liviano, usado en Void Linux             |
| SysVinit   | Init tradicional basado en scripts estilo SYSV |
| s6         | Init moderno y minimalista                   |

## 4. Librería C estándar (libc)

Provee las funciones base del sistema (malloc, open, read, printf, etc.). Toda aplicación enlaza contra ella.

- **glibc** — La más común, pesada pero completa (GNU C Library)
- **musl** — Alternativa ligera, enfocada en conformidad con estándares POSIX
- **uClibc-ng** — Para sistemas embebidos

La libc se ubica entre el kernel y los programas de usuario.

## 5. Terminal virtual (VT / TTY)

El kernel maneja las terminales virtuales (VT), pero no provee el proceso que las atiende. Aquí entra **Getty** (provisto por util-linux), que se encarga de:

- Gestionar el login en cada TTY (Ctrl+Alt+F1..F7)
- Proyectar la pantalla de inicio de sesión
- Proveer `getty` y `agetty`

```bash
ps aux | grep agetty  # Ver TTYs activas
```

## 6. Shell

Una vez autenticado, el usuario necesita un intérprete de comandos. La shell es la interfaz entre el usuario y el sistema operativo.

| Shell     | Descripción                              |
| --------- | ---------------------------------------- |
| Bash      | La más común, shell por defecto en GNU/Linux |
| Zsh       | Extensible con plugins y temas           |
| Fish      | Autosugerencias y resaltado integrado    |
| Dash      | Shell minimalista y rápida (usada en `/bin/sh` en Debian) |
| Ash       | Shell ligera para BusyBox               |

## 7. Toolbox (Herramientas básicas)

Las utilidades básicas del sistema (`ls`, `cp`, `mv`, `rm`, `cat`, `grep`, `sed`, `awk`, etc.) son provistas por:

- **GNU Coreutils** — La colección estándar en distribuciones Linux
- **BusyBox** — Versión todo-en-uno para sistemas embebidos (Router, Alpine Linux)
- **util-linux** — Utilidades del sistema como `fdisk`, `mount`, `kill`, `agetty`

## 8. Gestor de usuarios (Shadow)

El paquete **shadow** provee las herramientas para administrar usuarios y contraseñas:

- `useradd`, `usermod`, `userdel`
- `passwd`, `chage`
- Manejo de `/etc/shadow`, `/etc/passwd`, `/etc/group`

Es esencial para la seguridad y el control de acceso multiusuario.

## 9. Gestor de paquetes (Opcional pero esencial en la práctica)

Para instalar, actualizar y remover software se necesita un gestor de paquetes:

| Gestor       | Distribución                         |
| ------------ | ------------------------------------ |
| APT          | Debian, Ubuntu, Mint                 |
| DNF          | Fedora, RHEL                         |
| Pacman       | Arch Linux, Manjaro                  |
| Zypper       | openSUSE                             |
| Emerge       | Gentoo                               |
| Xbps         | Void Linux                           |

## Flujo de arranque resumido

```
BIOS/UEFI → Bootloader (GRUB) → Kernel → Init (PID 1) → Getty → Login → Shell
```

Cada componente depende del anterior. Si alguno falta, el sistema no arranca correctamente.
