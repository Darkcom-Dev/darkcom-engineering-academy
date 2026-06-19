# Fundamentos del Shell en Linux

## Shells y sus características

| Shell | Extensión | Descripción |
|-------|-----------|-------------|
| Bourne shell | `*.sh` | Shell original de Unix |
| C Shell | `*.csh` | Shell con sintaxis similar a C |
| TC Shell | `*.tcsh` | Superconjunto de C Shell |
| Korn Shell | `*.ksh` | Shell compatible con Bourne con mejoras |
| Bourne Again Shell | `*.bash` | Shell por defecto en la mayoría de distribuciones Linux |

## Tipos de shell

- **Login shell**: se inicia al iniciar sesión en el sistema.
- **Non-login shell**: se inicia sin autenticación (ej. terminal dentro de una sesión gráfica).

## Invocación del shell

`/etc/profile` → `~/.bash_profile` → `~/.bashrc` → `/etc/bashrc` → `~/.bash_logout`

## Comandos útiles

```shell
cat /etc/shells        # Lista los shells instalados en el sistema
echo $0                # Muestra el shell actual
echo $SHELL            # Ruta del shell por defecto
echo $PATH             # Directorios donde se buscan ejecutables
printenv               # Muestra todas las variables de entorno
```

### Ejemplo de `/etc/shells`

```
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
```

## Variables

Existen dos tipos de variables:

- **Variables de shell**: afectan únicamente al shell actual. Se asignan dentro de un script:
  ```shell
  var="abc"
  echo $var   # >>> abc
  ```

- **Variables de entorno**: afectan a todos los shells y procesos del sistema. Se consultan con `printenv` o `env`.

## Comentarios

```shell
# Comentario de una sola línea
```

```shell
: '
Este sería un comentario
multilínea
'
```

## HereDocs

Los HereDocs permiten pasar múltiples líneas de texto como entrada a un comando:

```shell
cat << DELIM
Agregue su texto
múltiples líneas de texto
DELIM
```