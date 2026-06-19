# Colores en la terminal

> Escrito por: Braulio

Los colores en la terminal se definen mediante **códigos ANSI**, que son secuencias de escape especiales. Aunque es un estándar antiguo, sigue siendo compatible con todas las terminales modernas.

## Comandos útiles

- Para saber qué tabla de colores soporta tu terminal: `echo $TERM` (ej: `xterm-256color`)
- Para ver la lista de colores usada por `ls`: `echo $LS_COLORS` (variable de entorno usada por `ls` y visible con `printenv`)

## Secuencias de escape

| Código  | Efecto      | Ejemplo                    |
| ------- | ----------- | -------------------------- |
| `00`    | Ninguno     | `printf '\033[0m'`         |
| `01`    | Negrita     | `printf '\033[1m'`         |
| `03`    | Cursiva     | `printf '\033[3m'`         |
| `04`    | Subrayado   | `printf '\033[4m'`         |
| `05`    | Parpadeo    | `printf '\033[5m'`         |
| `07`    | Invertido   | `printf '\033[7m'`         |
| `08`    | Oculto      | `printf '\033[8m'`         |
|         | Reiniciar   | `printf '\e[0m'`           |

> **Nota:** `\033[` (ANSI C/C++), `\e[` (bash) y `\x1b[` (Python/Node.js) son equivalentes.

Es posible combinar varios efectos con `;`. Ejemplo:

```bash
printf '\e[1;5;7;92m'  # negrita;parpadeo;invertido;verde claro
```

## Colores básicos (16 colores)

| Color       | Frente     | Fondo      |
| ----------- | ---------- | ---------- |
| Negro       | `\033[30m` | `\033[40m` |
| Rojo        | `\033[31m` | `\033[41m` |
| Verde       | `\033[32m` | `\033[42m` |
| Naranja     | `\033[33m` | `\033[43m` |
| Azul        | `\033[34m` | `\033[44m` |
| Magenta     | `\033[35m` | `\033[45m` |
| Cyan        | `\033[36m` | `\033[46m` |
| Gris claro  | `\033[37m` | `\033[47m` |
| *Default*   | `\033[39m` | `\033[49m` |

## Colores brillantes (16 colores extendidos)

| Color       | Frente      | Fondo       |
| ----------- | ----------- | ----------- |
| Gris oscuro | `\033[90m`  | `\033[100m` |
| Rojo claro  | `\033[91m`  | `\033[101m` |
| Verde claro | `\033[92m`  | `\033[102m` |
| Amarillo    | `\033[93m`  | `\033[103m` |
| Azul claro  | `\033[94m`  | `\033[104m` |
| Magenta cl. | `\033[95m`  | `\033[105m` |
| Cian claro  | `\033[96m`  | `\033[106m` |
| Blanco      | `\033[97m`  | `\033[107m` |
