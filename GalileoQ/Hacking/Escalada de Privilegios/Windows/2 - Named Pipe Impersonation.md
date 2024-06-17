Un _**named pipe**_ es una técnica que tiene el sistema operativo _**Windows**_ para facilitar la comunicación entre procesos. Por lo que, si se mira desde el punto de vista de un _**pentester**_, ésta puede ser utilizada para lograr un objetivo como el de ejecutar un código en un contexto privilegiado.

--------------------

Por ejemplo, si intentamos acceder a la flag, vemos que no tenemos permisos y debemos escalar privilegios:
![[Pasted image 20240612161022.png]]

Y con el comando load incognito y después list_tokens -u podemos ver que el token del usuario administrador está disponible:
![[Pasted image 20240612161026.png]]

Y con el comando impersonate_token podemos pasar a ser el usuario administrador:
![[Pasted image 20240612161032.png]]

Y ya accedemos dentro del directorio del usuario administrador:
![[Pasted image 20240612161052.png]]
