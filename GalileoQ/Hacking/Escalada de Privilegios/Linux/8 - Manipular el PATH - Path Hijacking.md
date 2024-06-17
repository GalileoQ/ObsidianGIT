Podemos escalar privilegios en Linux manipulando el path si vemos que la máquina ejecuta alguna herramienta, por ejemplo curl, ifconfig, etc. Por tanto podemos exportar una variable de entorno con un /bin/bash y luego cuando el sistema ejecute el ifconfig o curl se ejecutará el /bin/bash:

[[MAQUINA KENOBI (Recursos compartidos SMBMAP, monturas con showmount, vulnerabilidad proFTPd para copiar el id_rsa a la montura en máquina loca, escalada privilegios manipular path)]]

Si lo ejecutamos vemos que se trata de una especie de menú que interactúa con nosotros y nos proporciona información del sistema utilizando el comando curl:
![[Pasted image 20240612155949.png]]

Por tanto, podemos modificar la variable de entorno en curl y añadirle que ejecute el comando /bin/bash cada vez que la máquina ejecute el comando curl, por tanto insertamos el comando /bin/bash dentro del archivo curl, y lo añadimos como variable de entorno, para que el menú lo ejecute y nos devuelva esa bash como root:
![[Pasted image 20240612155957.png]]

De esta forma, como vemos, lo que hemos enviado se ejecutará como variable de entorno cada vez que la máquina ejecute algún comando:
![[Pasted image 20240612160001.png]]

De hecho con el comando ifconfig también podríamos haber escalado privilegios de la misma forma, ya que la opción 3 ejecuta el ifconfig:
![[Pasted image 20240612160011.png]]
## OTRO EJEMPLO

[[MAQUINA FRIENDLY 2]]

Vamos a suponer que ejecutando el comando sudo -l podemos ver que podemos ejecutar el script security.sh:
![[Pasted image 20240612160018.png]]

Dentro del contenido del script vemos que se ejecuta el comando tr, por lo que es posible que podamos hacer un path hijacking con tr o con grep:
![[Pasted image 20240612160023.png]]

Por tanto creamos un archivo llamado grep donde se ejecute un comando como root, el cual será cambiar los permisos a la bash:
![[Pasted image 20240612160027.png]]

Y ahora enviamos esto al path:
![[Pasted image 20240612160030.png]]

Ahora ejecutamos el script como root pasándole el PATH:
![[Pasted image 20240612160032.png]]