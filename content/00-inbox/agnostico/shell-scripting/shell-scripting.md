# Shell scripting

No es un lenguaje de programación como tal, mas bien es un Frankestein de aplicaciones que simulan funcionar como un lenguaje de programación, pero lo que realmente hay bajo el capó son pequeños programas bajo la filosofía Unix compilados generalmente en C o Rust que al ejecutarse aprovechan la salida a modo de variables y funciones.

Esta forma de programar es aprovechada principalmente por hackers, devops e ingenieros de sistemas. Es una forma muy practica de programar todo aquello que automatice procesos por fuera de lo que es un proyectos de programación serio.

Para aprender a programar, debes tener nociones del [[uso-terminal-basico-linux]]

## Relacionados:
- [[01-variables-y-tipos-de-datos]]
- [[02-funciones-en-bash]]
- [[03-comentarios-y-documentacion]]
- [[04-operaciones-con-archivos-y-directorios]]
- [[05-comunicacion-entre-scripts]]
- [[06-entrada-y-salida-estandar]]
- [[07-redirecciones-y-manejo-de-errores]]


## Debugging

```sh
#!/bin/bash -x
# Todo script de shell debe empezar con la linea de arriba
# el exclamativo'!' le da el poder de ejecucion a cada uno de los comandos
# escritos aqui, /bin/bash viene siendo la ruta del programa que ejecuta el
# shell

# La segunda regla es que cada nueva linea es un nuevo comando
# Trate de no combinar comandos en una sola linea y tampoco de separar un 
# comando en varias lineas. lo mismo pasa con if/then/else/ aunque sea
# tentador escribir en la misma linea, debe hacerse en lineas separadas. 
# nano tiende a envolver las lineas en un renglon extra, para evitar esto
# use ALT + L para desactivar el envoltorio de texto.

# Comente con el caracter '#'

# Los comandos pueden ser envueltos dentro de parentesis '()' para evitar que
# se rompa o se mezcle con otros comandos en la misma linea

date +%m_%d_%y-%H.%M.%S

# como hacer una variable: variable=$(comando -argumento)
# no puede haber espacio siempre va pegado variable=$()

fecha_formateada=$(date +%m_%d_%y-%H.%M.%S)

# Para usar la variable: $variable

#echo 'Esta es la fecha formateada: ' $fecha_formateada

cp -iv "$1" "$2".$fecha_formateada

# cp es un comando para copiar y usa 2 argumento origen destino representados
# en $1 como origen y $2 como destino.

# https://www.howtogeek.com/67469/the-beginners-guide-to-shell-scripting-the-basics/

# https://www.howtogeek.com/68184/the-beginners-guide-to-shell-scripting-2-for-loops/
```

## Error handling

```sh

#!/bin/bash

ssh darkcom@10.206.34.56
if [ $? -eq 0 ]
then 
	echo "Conectado a la maquina remota"
else
	echo "Error: no hay respuesta por parte de ma la maquina"
fi
```

## Log utils

```sh
#!/bin/bash

function my_log(){
	echo -e "$1" | tee -a $LOGFILE
	}
	
function my_out(){
	echo -e "$1" | tee -a $LOGFILE $OUTFILE
	}
```

## Menú esperando tecla

```sh

#!/bin/bash

# Espere por la entrada
echo "Presione una tecla para continuar..."
while [ true ]
do
	# Lee cada 3 segundos
	read -t 3 key
	if [ $? -eq 0 ]
	then
		echo "Usted preguntó por terminar el script"
		exit
	else
		echo "Esperando por la entrada del usuario, presione cualquier tecla"
	fi
done
```

## Simple menu

```sh
#!/bin/bash

# Usando un menú profesional en bash script
echo "Seleccione opcion de compra"
select cart in amazon flipkart local
do
	echo "Usted seleccionó: $cart"
	case $cart in 
	amazon)
		echo "Vamos por amazon";;
	flipkart)
		echo "Vamos por plipkart";;
	local)
		echo "Vamos por local";;
	*)
		echo "Error: Por favor elija una opcion entre 1-3";;
	esac
done
```

## config parsing

```sh
#!/bin/bash

MYCONFIG='my_config.cfg'

function parse_config(){
	CONFIG=$1
	echo "Leyendo el archivo de configuracion $CONFIG"
	cat $CONFIG | cut -d"=" -s -f1,2 > /tmp/temp.cfg
	source /tmp/temp.cfg
	}
	
parse_config $MYCONFIG

echo "Executing my application..."
echo "==========================="
echo "User : $user"
echo "Applicacion : $app"
echo "Home directory : $location"
echo "Hostname : $host"
```
