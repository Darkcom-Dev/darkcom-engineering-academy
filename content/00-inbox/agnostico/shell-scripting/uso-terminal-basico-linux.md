# Uso de terminal básico en Linux
## Linux Commands

- [[file-commands-cheat-sheet]]
- [[gestion-de-procesos-en-linux]]
- [[comandos-de-red-en-linux]]

## Propias del shell

![[vista-previa-ficheros.jpg]]

### Comodines

\* = Supongamos algunos nombres de archivos

```bash
- trust_wallet.txt
- trigo.txt
- technologies
```

como comando puedo escribir tr*, el seleccionará trust y trigo

como comando puedo escribir *.txt y selecionará todo los archivos terminados en txt

? = busca un nombre con cualquier caracter, supongamos algunos nombres de archivo.

```bash
- borrador12.txt
- borrador23.doc
- borrador4.txt
- borrador15.odp
```

- si se escribe borrador?.txt devolverá borrador4.txt
- si borrador??.txt devolverá borrador12.txt
- si se combina borrador??.* devolverá borrador12.txt, borrador23.doc y borrador15.odp


## Otros comandos utiles

- cal = imprime en pantalla un calendario.
- echo [texto] >> [fichero] = agrega una linea nueva a un archivo.
- \> redirecciona o escribe la salida de un comando a un archivo de texto.
- \>\> adiciona contenido a un archivo de texto ya existente.


## Manejo de rutas y archivos

### Cambio de directorio 

- `cd` change directory: cambia de directorio. usar CD solo, te lleva a la carpeta de usuario ~
- `cd ..` te retorna a un directorio de nivel mas alto.
- `cd .` te retorna la carpeta actual.
- `cd ../../x` te mueve a un directorio dos niveles por encima y luego va al directorio X

### Listado de ficheros y directorios

- `ls =` listar archivos y directorios de la ruta actual.
- `ls -l` lista con detalles.
- `ls -a` muestra archivos ocultos.
- `ls -S` ordena por tamaño.
- `ls -t` ordena por fecha.
- `ls -r` ordena cualquier cosa en el orden inverso.
- `ls -laSr` = `ls -l -a -S -r` se puede combinar varios argumentos con un gion "-"

### Crear directorio

- `mkdir` make directory = crea carpetas o directorios.
- `mkdir -P` crea directorios incluido las rutas inexistentes.

### Crear fichero

- `touch` crea ficheros o archivos.

### Eliminar fichero.

- `rm [fichero]` elimina ficheros.
- `rm [directorio] -r` elimina directorios.

### Mover fichero.

- `mv [fichero] [directorio]` mueve el ficheros hacia el directorio.
- `mv [fichero] [nombre]` renonbra el fichero, si dicho nombre no correscponde al de un directorio.
- `mv [directorio] -r` mueve directorios o los renombra.

### Copiar fichero.

- `cp [fichero o directorio]` copia directorios o ficheros.
>[!Important]
> https://www.guia-ubuntu.com/index.php/%C2%BF_C%C3%B3mo_cambiar_o_asignar_la_contrase%C3%B1a_del_superusuario_(root)_%3F


## Ejercicios:

Utilizando la estructura de ficheros que se adjunta, realizar los siguientes ejercicios:

- [ ] Copiar el fichero borrador-datos-finales.xls a datos-finales-copia.xls
- [ ] Renombrar pescando.jpg a lubina.jpg
- [ ] Mover estadisticas/ a ficheros/documentos/
- [ ] Borrar todas las canciones de The Corrs
- [ ] Mover todas las fotos en personales/ a un nuevo directorio llamado ficheros/vacaciones/
- [ ] Borrar todo lo que contiene el directorio musica/2009/
- [ ] Mover todos los ficheros con extensión .gif del directorio fotos/ al directorio ficheros/

Copiar todos los ficheros que empiecen por m del directorio `/var/log/` a ficheros/

*Nota:*  También pueden ejecutar la siguiente instrucción para poder descargar el fichero desde cualquier terminal que tenga conexión a internet `wget bit.ly/2skfWSI -O ficheros.tar.gz`

Una vez descargado el archivo ficheros.tar.gz, puede descomprimirlo usando `tar -xzf ficheros.tar.gz.` Esto creará un directorio llamado "ficheros" con el contenido que se puede ver en el fichero adjunto llamado vista previa.

-----
## Administración de usuarios.

- `su` Super user: aumenta los privilegios y el control del usuario al convertirse en administrador.
- `who` Me dice que session, en que terminal estoy y una fecha.
- `whoami` Me dice que usuario soy.
- `groups` me dice a que grupos de usuario pertenezco.
- `id` devuelve id del usuario, id del grupo y de los grupos al que pertenece el usuario.
- `adduser [usuario nuevo]` Crea un nuevo usuario completo con grupo, contraseña y datos administrativos.~No usar~
- `addgroup [Nuevo grupo]` Crea un nuevo grupo.
- `usermod -g [nombre grupo] [usuario]` modifica el grupo principal de un usuario.
- `passwd [usuario]` modifica la contraseña de un usuario, solo puede ser modificado por el propio usuario o por root.

### Permisos de ficheros chmod

cada fichero tiene letras de permisos que se pueden ver al ejecutar ls -l

	- : sin permiso
	r : permite leer
	w : permite escribir
	x : permite ejecutar

Estos vienen en 3 grupos de 3 letras cada uno:

	El primer grupo : son los permisos de usuario.
	El segundo grupo : son los permisos del grupo.
	El tercer grupo : son los permisos de otros grupos.

Hay un caracter antes de los grupos de permisos:

	- : fichero regular.
	d : fichero directorio.
	l : link o acceso directo.

En un directorio, las letras **rwx** tienen otro significado:

	r : permite listar el contenido de la carpeta.
	w : permite crear o eliminar ficheros dentro de la carpeta.
	x : permite acceder a la carpeta.

- `chmod [parametro] [nombre fichero]` modifica los permisos de los ficheros.

	- \+ : agrega un permiso
	- \- : retira un permiso
	- = : asigna permisos
	- u : refiere al usuario
	- g : refiere al grupo
	- o : refiere a los otros

- `chmod u+r,g-w,u=rwx fichero.txt` agrega permiso de lectura al usuario, retira permiso de escritura al grupo y establece permisos a los otros grupos.


-----

## Administracion de software

Para instalar algun programa o libreria: `sudo apt = aptitude get install x`, estas librerias o programas estan alojados en servidores comunmente llamados **repositorios**, cada distribucion de linux tiene sus propios repositorios, incluso algunos programas tienen su repositorio oficial. 

Existe un fichero que podemos modificar para agregar o remover las direcciones de los repositorios `/etc/apt/sources.list` , pero para modificarlo se necesitan permisos de administrador.

```sh
apt-update # actualiza los paquetes.
apt-upgrade # instala las actualizaciones de los paquetes-
apt-cache search [programa] # busca en los paquetes descargados alguno en particular.
apt-get install [programa] # instala un paquete, primero lo descarga y luego lo instala.
apt-get remove [programa] # desisntala un paquete o programa.
apt-get purge [programa] # elimina los rastros que deja un programa.
```
apt search [programa] # mira el estado del paquete de un programa, si deja rastros o no. Para esto el comando muestra una serie de letras que hay que entender:

- nota: El comando aptitude funciona para distros muy viejas.
```
aptitude # (sin argumentos) Abre una interfaz de consola que ayuda mucho para instalar, eliminar o actualizar paquetes. (muy util)

c # dejó rastros de configuracion osea basura.
p # no esta instalado pero el paquete está disponible para instalarse.
i # el paquete está instalado.
v # que está instalado en un entorno virtual
a # que se instaló de forma automatica.
```
- Nota: Se recomienda el gestor de paquetes synaptic para instalar y desistalar pero con interfaz grafica del sistema.

### Limpieza de archivos de instalacion.

```sh
apt # Advanced Packging Tool
apt-get autoclean 	# elimina del cache paquetes descargados.
apt-get clean 		# elimina todos los paquetes descargados que no han sido instalados.
apt-get autoremove 	# elimina paquetes huerfanos o dependencias que quedan instaladas despues de haber hecho una instalacion o haberla eliminado
```

>[!Nota]
>https://www.softzone.es/2019/06/21/web-gratis-distro-linux-navegador/

### ¿Quieres saber donde queda instalado una aplicacion en linux?

```sh
which [programa] # Muestra el primer binario encontrado con el nombre del programa en las variables definidas en el entorno PATH
wherein [programa] # Similar al anterior pero incluye directorios y ficheros coinciden en el nombre mostrando sus rutas.
find [programa] # similar a los anteriores, pero busca por fuera del entorno PATH
sudo find /usr -wholename '*/bin/postgres'
```

https://www.sysadmit.com/2017/09/linux-como-saber-donde-esta-instalado-un-programa.html

-----

## ¿Como obtener informacion del hardware?

```bash
free -m # Cantidad de RAM utilizada, libre y total en el sistema con el comando free.

df -H 	# Informa sobre varias particiones, sus puntos de montaje y el espacio utilizado y disponible en cada uno.

sudo fdisk -l # es una utilidad para modificar particiones en discos duros, y también se puede usar para listar la información de la partición.

mount | column -t # Montar / desmontar y ver sistemas de archivos montados.

glxinfo # Informacion de la tarjeta grafica. -B da info basica de la tarjeta.
glxgears # muestra unos piñones girando mientras toma info de la cantidad de FPS.
sudo prime-select query = indica cual es el primer controlador de video seleccionado
sudo prime-select nvidia = selecciona a la fuerza un controlador en este caso nvidia
```

### Listas

### lshw – Lista de hardware en Linux
Esta utilidad de propósito general nos brinda **información breve y detallada** sobre múltiples unidades de hardware en Linux, como CPU, memoria, disco, controladores usb, adaptadores de red, etc. Lshw extrae la información de diferentes **/proc files**.

`sudo lshw -c processor # El programa lshw nos aporta información fácil de leer y comprender`, igual que el método anterior (inxi). Tiene varias opciones, por lo que si queremos obtener los datos de la CPU, tenemos que ejecutar el siguiente comando con permisos de superusuario:


### lspci - Listado de hardware a traves de puertos PCI

Muestra info de todos los cpus, gpus, controladora de audio, controladora de modem, etc. 

- El comando lspci enumera todos los buses pci y detalles sobre los dispositivos conectados a ellos.
- El adaptador vga, la tarjeta gráfica, el adaptador de red, los puertos usb, los controladores sata, etc. caen dentro de esta categoría.

```sh
lspci -knn # muestra dispositivos pci y los drivers que los controlan e IDs
lspci -v -s 
lspci | awk '/VGA/{print $1}'
```

### lsscsi - Listar dispositivos scsi. 

Enumera los dispositivos scsi / sata, como los discos duros y las unidades ópticas.

### lscpu - informcación de la CPU
- El comando lscpu es uno de los más comunes y utilizados para obtener información de la CPU:
- El comando lscpu informa sobre la CPU y las unidades de procesamiento, una de las partes mas importantes del hardware en Linux.
- El comando no tiene más opciones o funcionalidades.

### lsusb - Lista de los buses usb y detalles del dispositivo.

- Este comando muestra los controladores USB y detalles sobre los dispositivos conectados a ellos.
- Por defecto, se imprime una breve información.
- Si queremos la opción detallada utilizamos el argumento «-v» para imprimir información mas explicita sobre cada puerto usb.

### lsblk - lista de dispositivos de bloque. 

Enumerar la información de todos los dispositivos de bloque, que son las particiones de disco duro y otros dispositivos de almacenamiento como unidades ópticas y unidades de memoria flash.

lsdev
lspcmia
lsmod = Muestra el estado de los modulos del kernel
dkms status = muestra tarjetas graficas.

### inxi - 

Inxi es un script mega bash de 10K líneas que obtiene detalles de hardware de múltiples orígenes y comandos diferentes en el sistema, y ​​genera un hermoso informe que los usuarios no técnicos pueden leer fácilmente.
`inxi -Gx # da informacion de la grafica, de forma contundente.`

-----

## Obtencion por medio de archivos de la carpeta PROC

Muchos de los archivos virtuales en el directorio /proc contienen información sobre hardware en Linux y configuraciones. Éstos son algunos de ellos:

Información de CPU / memoria:

- El método más simple es ver el contenido de /proc/cpuinfo. Es un archivo virtual que nos muestra la configuración de la CPU. Con este archivo conoceremos el número de cores, modelo de la CPU, tamaño de caché, etc. `$ cat /proc/cpuinfo`
- Informacion de la cpu `cat /proc/cpuinfo`
- Informacion de la memoria `cat /proc/meminfo`
- Informacion de Linux y el kernel: `cat /proc/version`
- Informacion de Dispositivos Sata / SCSI `$ cat /proc/scsi/scsi`
- Informacion de particiones `cat /proc/partitions`
- Este funciona para obtener  información sobre dispositivos sata como los discos duros. `sudo hdparm -i /dev/sda`

(Saber que driver usa)[https://www.linuxito.com/gnu-linux/nivel-medio/317-como-saber-que-driver-esta-utilizando-un-dispositivo-de-hardware-en-gnu-linux]

or more detailed technical information, including how to use the chroot command to gain access to a broken Ubuntu system’s files and restore GRUB2, consult the Ubuntu wiki.

```sh
sudo cfdisk # muestra o manipula la tabla de particiones, 
# permite saber si la particion es GPT o MBR o cualquier otro
sudo gdisk [ruta disco] # modifica o convierte la particion de MBR a GPT
```

------

## Comandos para automatizar tareas.

### cron o crontab

Ejecuta un comando o un script un dia determinado a una hora determinada, es necesario escribir 6 argumentos.
	0-59: Minuto
	0-23: Hora
	1-31: Dia
	1-12: Mes
	0-6: Dia de la semana, Domingo es = 0
	Argumento o comando o script a ejecutar. 
- Para programar una tarea escribe: `crontab -e` 
- Te pide escoger un editor de texto si es la primera vez que lo usas, en dicho documento hay instrucciones precisas de como programar una tarea con un ejemplo.
- Introduce la periodicidad y luego el comando con la ruta larga de donde se encuentra el script de shell

- Para listar tareas `crontab -l`
- Para eliminar tareas `crontab -r`

- Para ver el historial de mensajes `ps -ef | grep cron | grep -v grep`
- Si tienes problemas con los mensajes por mail, instala: `sudo apt install postfix` si el mensaje es "No MTA Installed"
- Para ver los mensajes de log que deja cron `grep CRON /var/log/syslog`
- Para mirar los mensajes de correo que deja cron `sudo tail -f /var/mail/darkcom`

[https://qastack.mx/server/449651/why-is-my-crontab-not-working-and-how-can-i-troubleshoot-it]



### at

### timer =


