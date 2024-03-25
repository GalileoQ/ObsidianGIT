Ahora para escalar los privilegios, vamos a hacer una enumeración de los procesos del sistema con el comando ps, donde vemos uno que se llama spoolsv, el cual es vulnerable:

![[Pasted image 20230314112913.png]]
Si buscamos en google como escalar privilegios explotando el servicio spoolsv, nos encontramos con este repositorio de github y con la vulnerabilidad pública:
![[Pasted image 20230314113242.png]]
![[Pasted image 20230314113254.png]]
Y nos dice que debemos ejecutar este script dentro de la máquina víctima:
![[Pasted image 20230314113349.png]]
Por tanto vamos a clonarnos este repositorio y lo vamos a compartir con la máquina víctima con impacket-smbserver para poder ejecutarlo allí:
![[Pasted image 20230314113827.png]]
Y lo descargamos en la máquina víctima:
![[Pasted image 20230314113844.png]]
Y ahora vamos a ejecutarlo siguiendo las instrucciones del repositorio de github, pero al ejecutar el primer comando nos lanza un error:
![[Pasted image 20230314114117.png]]
Por tanto vamos a mirar por google como quitar esta restricción y poder ejecutar este script; y nos encontramos el link a stackoverflow:
![[Pasted image 20230314114256.png]]
![[Pasted image 20230314114311.png]]
Por tanto vamos a ejecutar este comando dentro de la víctima y ver si así ya se nos arregla el error; y vemos que ahora ya no nos sale ese error:
![[Pasted image 20230314114441.png]]
Seguimos ejecutando el resto de los comandos del script:
![[Pasted image 20230314114607.png]]
![[Pasted image 20230314114654.png]]
Ahora vamos a ver el usuario que nos ha creado este script utilizando el comando net user, para ver sus características; y vemos que se encuentra dentro del grupo de administrador:
![[Pasted image 20230314114841.png]]
Ahora que ya tenemos un nuevo usuario con permisos de administrador, vamos a conectarnos a este nuevo usuario otra vez con la herramienta evil-winrm; y vemos que se conecta bien:
![[Pasted image 20230314115112.png]]
Y vemos que con este nuevo usuario podemos entrar dentro de la carpeta del Administrador y acceder a la flag de root:
![[Pasted image 20230314115214.png]]

