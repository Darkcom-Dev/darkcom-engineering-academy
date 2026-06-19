# Variables y Tipos de Datos en Bash

Las variables son fundamentales en cualquier lenguaje de programación, y en los scripts de shell no son la excepción. Nos permiten almacenar y manipular datos durante la ejecución de nuestro script.

## Declaración y Uso Básico de Variables

En bash, las variables se declaran simplemente asignando un valor a un nombre. No es necesario declarar su tipo explícitamente.

```bash
# Declaración de variables
NOMBRE="Juan Carlos"
EDAD=30
CIUDAD="Madrid"
PRECIO=19.99
ACTIVO=true

# Uso de variables
echo "Mi nombre es $NOMBRE"
echo "Tengo $EDAD años"
echo "Vivo en $CIUDAD"
echo "El precio es: $PRECIO euros"
echo "¿Está activo? $ACTIVO"
```

### Reglas para Nombrar Variables
- Los nombres pueden contener letras, números y guiones bajos
- No pueden comenzar con un número
- Por convención, se usan mayúsculas para variables de entorno y variables globales
- Son sensibles a mayúsculas y minúsculas (`nombre` y `Nombre` son diferentes)

## Variables del Sistema

Bash proporciona varias variables predefinidas que contienen información útil sobre el entorno:

```bash
# Información del usuario y entorno
echo "Usuario actual: $USER"
echo "ID de usuario: $UID"
echo "Directorio home: $HOME"
echo "Shell actual: $SHELL"
echo "Tipo de terminal: $TERM"
echo "Directorio de trabajo actual: $PWD"

# Información del sistema
echo "Sistema operativo: $OSTYPE"
echo "Arquitectura: $MACHTYPE"
echo "Hostname: $HOSTNAME"

# Variables de PATH y localización
echo "Ruta de búsqueda: $PATH"
echo "Idioma local: $LANG"
```

## Sustitución de Parámetros

Bash ofrece varias formas de manipular variables mediante sustitución de parámetros:

```bash
NOMBRE_COMPLETO="Juan Carlos Pérez García"
CIUDAD_ORIGEN="  Madrid  "  # Con espacios

# Longitud de una variable
echo "Longitud del nombre: ${#NOMBRE_COMPLETO}"  # Resultado: 22

# Extraer subcadenas
echo "Primeros 5 caracteres: ${NOMBRE_COMPLETO:0:5}"  # Resultado: Juan C
echo "Desde posición 6, 4 caracteres: ${NOMBRE_COMPLETO:6:4}"  # Resultado: Carlos

# Eliminar patrones
echo "Sin apellidos: ${NOMBRE_COMPLETO%% *}"  # Resultado: Juan
echo "Sin nombre: ${NOMBRE_COMPLETO#* }"  # Resultado: Carlos Pérez García
echo "Solo primer apellido: ${NOMBRE_COMPLETO% *}"  # Resultado: Juan Carlos Pérez

# Reemplazar contenido
echo "${NOMBRE_COMPLETO/Juan/María}"  # Resultado: María Carlos Pérez García
echo "${NOMBRE_COMPLETO// /_}"  # Resultado: Juan_Carlos_Pérez_García

# Valores por defecto
SIN_DEFINIR=
echo "Valor de SIN_DEFINIR: ${SIN_DEFINIR:-'No definido'}"  # Resultado: No definido
echo "Valor de SIN_DEFINIR: ${SIN_DEFINIR:-'Asignado ahora'}"  # Resultado: Asignado ahora
echo "Valor de SIN_DEFINIR después: $SIN_DEFINIR"  # Todavía está vacío

# Asignar si no está definido
: "${APELLIDO:=Pérez}"  # Solo asigna si APELLIDO no está definido
echo "Apellido: $APELLIDO"  # Resultado: Pérez
```

## Lectura de Entrada del Usuario

Para obtener datos del usuario durante la ejecución del script:

```bash
# Lectura simple
read -p "Ingresa tu nombre: " NOMBRE
echo "Hola, $NOMBRE"

# Lectura con tiempo límite
if read -t 5 -p "Responde rápidamente (5 segundos): " RESPUESTA; then
    echo "Tu respuesta fue: $RESPUESTA"
else
    echo "\nTiempo agotado"
fi

# Lectura segura (para contraseñas)
read -s -p "Introduce tu contraseña: " CONTRASENA
echo  # Para nueva línea después de la entrada silenciosa
echo "Contraseña recibida (no se mostrará por seguridad)"

# Lectura de múltiples valores
read -p "Ingresa tres números separados por espacios: " A B C
echo "Los números son: $A, $B y $C"
echo "Su suma es: $((A + B + C))"
```

## Arreglos (Arrays)

Bash soporta arreglos indexados y asociativos:

```bash
# Arreglos indexados
FRUTAS=("Manzana" "Banana" "Naranja" "Pera")
echo "Primera fruta: ${FRUTAS[0]}"  # Resultado: Manzana
echo "Todas las frutas: ${FRUTAS[@]}"  # Resultado: Manzana Banana Naranja Pera
echo "Número de frutas: ${#FRUTAS[@]}"  # Resultado: 4

# Añadir elementos
FRUTAS+=("Sandía" "Uva")
echo "Después de añadir: ${FRUTAS[@]}"

# Recorrer un arreglo
echo "Listando frutas:"
for fruta in "${FRUTAS[@]}"; do
    echo "- $fruta"
done

# Arreglos asociativos (requiere bash 4.0+)
declare -a EDAD_FAMILIA
EDAD_FAMILIA["Juan"]=30
EDAD_FAMILIA["María"]=28
EDAD_FAMILIA["Pedro"]=5

echo "Edad de Juan: ${EDAD_FAMILIA[Juan]}"  # Resultado: 30
echo "Todos los nombres: ${!EDAD_FAMILIA[@]}"  # Resultado: Juan María Pedro
echo "Todas las edades: ${EDAD_FAMILIA[@]}"  # Resultado: 30 28 5
```

## Buenas Prácticas

1. **Usa comillas dobles** alrededor de las variables para evitar problemas con espacios y caracteres especiales:
   ```bash
   # Peligroso si NOMBRE contiene espacios
   saludo="Hola $NOMBRE"
   
   # Seguro
   saludo="Hola $NOMBRE"
   ```

2. **Nombra tus variables de manera descriptiva** pero concisa:
   ```bash
   # Mejor
   NOMBRE_USUARIO="Juan Carlos"
   
   # Evitar nombres genéricos como var1, var2, etc.
   ```

3. **Inicializa siempre tus variables** antes de usarlas:
   ```bash
   CONTADOR=0  # En lugar de asumir que empieza en 0
   ```

4. **Usa variables en mayúsculas solo para variables de entorno o configuración global**:
   ```bash
   # Variables de entorno o configuración
   RUTA_CONFIG="/etc/miapp/config.conf"
   
   # Variables locales de función o bloque
   total=0
   i=1
   ```

5. **Aprovecha la sustitución de parámetros** para evitar comandos externos innecesarios:
   ```bash
   # En lugar de: largo=$(echo "$TEXTO" | wc -c)
   largo=${#TEXTO}
   
   # En lugar de: extension=$(echo "$ARCHIVO" | awk -F. '{print $NF}')
   extension="${ARCHIVO##*.}"
   ```

## Ejercicios

1. Crea un script que solicite el nombre, edad y ciudad de una persona, y luego muestre un mensaje de presentación formateado.
2. Modifica un script existente para usar variables en lugar de valores hardcodeados.
3. Experimenta con la sustitución de parámetros para extraer el nombre de archivo y la extensión de una ruta completa.
4. Crea un script que use un arreglo para almacenar los días de la semana y muestre el día correspondiente a un número ingresado por el usuario.