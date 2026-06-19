# PWD

`pwd` : print working directory: mustra la ruta actual de la terminal.

**NOMBRE**
            pwd - imprime el nombre del directorio de trabajo actual (print working directory)
     
     **SINOPSIS**
            **pwd** [OPCION]...
     
     **DESCRIPCIÓN**
            Muestra el nombre de fichero completo del directorio de trabajo actual.
     
            **-L**, **--logical**
                   utiliza PWD del entorno, incluso si contiene enlaces simbólicos
     
            **-P**, **--physical**
                   evita todos los enlaces simbólicos
     
            **--help** muestra la ayuda y finaliza
     
            **--version**
                   muestra la versión del programa y termina
     
            Si no se especifica ninguna opción, se supone **-P**.
     
            NOTA:  su  shell puede tener su propia versión de pwd, que usualmente tiene prioridad sobre la versión que se describe aquí.
            Por favor acuda a la documentación de su shell para saber los detalles sobre las opciones que admite.