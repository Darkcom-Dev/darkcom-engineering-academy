# Funciones en Bash

Las funciones en bash permiten agrupar comandos bajo un nombre único para reutilizarlos múltiples veces dentro de un script. Mejoran la legibilidad, reducen la duplicación de código y facilitan el mantenimiento.

## Declaración y Sintaxis de Funciones

Hay dos formas sintácticas equivalentes para declarar una función en bash:

```bash
# Forma 1: usando la palabra clave function
function nombre_funcion {
    # cuerpo de la función
    comando1
    comando2
    # ...
}

# Forma 2: sintaxis directa (más portable a otros shells)
nombre_funcion() {
    # cuerpo de la función
    comando1
    comando2
    # ...
}
```

## Llamada a Funciones

Una vez declarada, una función se llama simplemente por su nombre:

```bash
# Declaración
saludar() {
    echo "Hola, ¿cómo estás?"
}

# Llamada
saludar  # Ejecuta el cuerpo de la función
```

## Parámetros en Funciones

Las funciones en bash pueden recibir parámetros, al igual que los scripts. Los parámetros se acceden usando las mismas variables especiales que para los argumentos del script:

| Variable | Descripción |
|----------|-------------|
| `$0` | Nombre del script (no cambia dentro de funciones) |
| `$1`, `$2`, ..., `$n` | Primer, segundo, ..., n-ésimo parámetro pasado a la función |
| `$@` | Todos los parámetros como palabras separadas |
| `$*` | Todos los parámetros como una sola palabra |
| `$#` | Número de parámetros pasados a la función |
| `${FUNCNAME[0]}` | Nombre de la función actual |

```bash
# Función que recibe parámetros
sumar() {
    # Verifica que se pasaron exactamente 2 parámetros
    if [ $# -ne 2 ]; then
        echo "Error: Se requieren exactamente 2 parámetros"
        return 1
    fi
    
    # Suma los parámetros
    RESULTADO=$(( $1 + $2 ))
    echo "La suma de $1 y $2 es: $RESULTADO"
    return 0
}

# Llamada con parámetros
sumar 5 3      # Salida: La suma de 5 y 3 es: 8
sumar 10 20    # Salida: La suma de 10 y 20 es: 30
sumar 5        # Salida: Error: Se requieren exactamente 2 parámetros
```

## Variables Locales y Globales

Por defecto, todas las variables en bash son globales. Para crear variables locales dentro de una función, se usa la palabra clave `local`:

```bash
# Variable global
CONTADOR_GLOBAL=0

contar() {
    # Variable local (solo visible dentro de esta función)
    local CONTADOR_LOCAL=0
    
    # Variable global (visible en todo el script)
    ((CONTADOR_GLOBAL++))
    
    # Incrementar y mostrar ambas
    ((CONTADOR_LOCAL++))
    echo "Dentro de la función:"
    echo "  Contador local: $CONTADOR_LOCAL"
    echo "  Contador global: $CONTADOR_GLOBAL"
}

# Llamadas a la función
contar  # Contador local: 1, Contador global: 1
contar  # Contador local: 1 (se reinicia), Contador global: 2

echo "Fuera de la función:"
echo "Contador global: $CONTADOR_GLOBAL"  # 2
# echo "Contador local: $CONTADOR_LOCAL"  # No imprime nada (variable no definida)
```

## Retorno de Valores

Las funciones en bash no pueden devolver valores complejos como en otros lenguajes, pero tienen varias formas de "retornar" información:

### 1. Código de Salida (Return Status)
El mecanismo más básico es usar `return` para devolver un código de estado (0-255), donde 0 indica éxito y cualquier otro valor indica error:

```bash
es_par() {
    if [ $(( $1 % 2 )) -eq 0 ]; then
        return 0  # Éxito (verdadero)
    else
        return 1  # Error (falso)
    fi
}

# Uso
es_par 4
if [ $? -eq 0 ]; then
    echo "4 es par"
else
    echo "4 no es par"
fi

es_par 5
if [ $? -eq 0 ]; then
    echo "5 es par"
else
    echo "5 no es par"
fi
```

### 2. Imprimir el Resultado (Recomendado)
La forma más común y flexible es hacer que la función imprima su resultado en stdout, y luego capturar esa salida con sustitución de comandos:

```bash
# Función que devuelve un valor mediante echo
obtener_dominio() {
    local email="$1"
    # Extrae el dominio de una dirección de email
    echo "${email#*@}"
}

# Capturar el resultado
MI_EMAIL="usuario@ejemplo.com"
DOMINIO=$(obtener_dominio "$MI_EMAIL")
echo "El dominio de $MI_EMAIL es: $DOMINIO"

# Función que realiza un cálculo
calcular_factorial() {
    local n=$1
    local resultado=1
    
    for (( i=2; i<=n; i++ )); do
        resultado=$(( resultado * i ))
    done
    
    echo $resultado
}

# Uso
FACTORIAL_5=$(calcular_factorial 5)
echo "5! = $FACTORIAL_5"  # 120
```

### 3. Variables de Referencia (Bash 4.3+)
En versiones más recientes de bash, se pueden usar "namerefs" para modificar variables externas:

```bash
modificar_variable() {
    local -n ref_var=$1  # Crea una referencia a la variable cuyo nombre se pasa como parámetro
    ref_var="Valor modificado"
}

MI_VARIABLE="Valor original"
echo "Antes: $MI_VARIABLE"

modificar_variable MI_VARIABLE
echo "Después: $MI_VARIABLE"  # Resultado: Valor modificado
```

## Alcance de Variables y Funciones

### Ámbito (Scope)
- Las variables declaradas sin `local` son globales y accesibles desde cualquier parte del script
- Las variables declaradas con `local` solo son visibles dentro de la función donde se declaran
- Las funciones son globales por defecto y pueden llamarse desde cualquier parte del script después de su declaración

### Orden de Declaración
Las funciones deben ser declaradas antes de ser llamadas:

```bash
# Esto funciona
mi_funcion() {
    echo "Dentro de la función"
}

mi_funcion  # Llamada correcta

# Esto NO funciona (función no declarada todavía)
otra_funcion

otra_funcion() {
    echo "Dentro de otra función"
}
```

## Funciones Recursivas

Bash soporta la recursión (funciones que se llaman a sí mismas):

```bash
# Factorial recursivo
factorial() {
    local n=$1
    
    # Caso base
    if [ $n -le 1 ]; then
        echo 1
        return 0
    fi
    
    # Llamada recursiva
    local prev=$(factorial $((n - 1)))
    echo $(( prev * n ))
}

# Uso
echo "5! = $(factorial 5)"  # 120
```

## Bibliotecas de Funciones

Para reutilizar funciones entre múltiples scripts, se pueden crear bibliotecas que se incluyen con `source` o `.`:

```bash
# Archivo: mi_biblioteca.sh
#!/bin/bash

# Funciones de la biblioteca
log_info() {
    echo "[INFO] $1"
}

log_error() {
    echo "[ERROR] $1" >&2
}

confirmar_accion() {
    local mensaje="${1:-¿Continuar? [s/N]: }"
    read -p "$mensaje" -n 1 -r
    echo    # Nueva línea
    [[ $REPLY =~ ^[SsYy]$ ]]
}

# Otro script que usa la biblioteca
#!/bin/bash
source ./mi_biblioteca.sh  # O: . ./mi_biblioteca.sh

log_info "Iniciando proceso..."
if confirmar_accion "¿Desea continuar? [s/N]: "; then
    log_info "Continuando con el proceso..."
else
    log_error "Operación cancelada por el usuario."
    exit 1
fi
```

## Buenas Prácticas

1. **Nombra tus funciones descriptivamente** usando verbos en infinitivo:
   ```bash
   # Bueno
   validar_entrada()
   procesar_archivo()
   enviar_notificacion()
   
   # Evitar nombres genéricos
   funcion1()
   procesar_datos()
   ```

2. **Mantén las funciones pequeñas y enfocadas** en una sola responsabilidad (principio de responsabilidad única)
3. **Documenta cada función** con comentarios que expliquen su propósito, parámetros y valor de retorno
4. **Usa variables locales** siempre que sea posible para evitar efectos secundarios no deseados
5. **Verifica los parámetros de entrada** al inicio de la función
6. **Devuelve códigos de salida significativos** (0 para éxito, otros valores para diferentes tipos de error)
7. **Prefiere imprimir resultados** en lugar de modificar variables globales cuando sea posible
8. **Usa `return` solo para códigos de estado**, no para devolver valores complejos
9. **Agrupa funciones relacionadas** en bibliotecas cuando vayan a reutilizarse
10. **Considera usar `set -euo pipefail`** dentro de funciones críticas para mejor manejo de errores

## Ejercicios

1. Crea una biblioteca de funciones para manejo de archivos que incluya:
   - `backup_file()`: crea una copia de seguridad de un archivo con timestamp
   - `ensure_dir()`: asegura que un directorio existe, creándolo si es necesario
   - `count_lines()`: cuenta las líneas de un archivo de manera eficiente
   - `find_largest()`: encuentra el archivo más grande en un directorio

2. Implementa una función recursiva para calcular la sucesión de Fibonacci.

3. Crea un script que use funciones para implementar una calculadora básica que soporte suma, resta, multiplicación y división.

4. Desarrolla una función que valide si una dirección de correo electrónico tiene un formato básicamente válido.