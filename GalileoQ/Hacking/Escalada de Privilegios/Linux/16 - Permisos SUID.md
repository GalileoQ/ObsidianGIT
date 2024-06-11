Estos permisos permiten que un archivo se ejecute con los mismos privilegios de seguridad del usuario que lo posee, en lugar de con los permisos del usuario que lo ejecuta.

Ejecutamos el comando find / -perm -4000 2>/dev/null para comprobar donde tenemos permisos SUID (Se tratan de permisos que permiten a un usuario ejecutar un archivo con los privilegios del propietario del archivo), por tanto ejecutamos este comando:
```bash
find / -perm -u=s -type f 2>/dev/null
```
![[Pasted image 20230306213232.png]]
El secreto aquí es buscar binarios de nombre extraño o binarios que generalmente no tengan el setuid activado; y salta a la vista el binario viewuser, por lo que lo vamos a inspeccionar:
![[Pasted image 20230306213415.png]]
Para este caso en particular, vamos a entrar dentro y nos encontramos con un error donde nos dice que no encuentra la carpeta /tmp/listusers, además de poder intuir que esta herramienta ejecuta el comando que se encuentre dentro del fichero testusers, por lo que podemos usarlo para ver la flag de root:
![[Pasted image 20230306213525.png]]

---------------------------------------------------------

## OTRO EJEMPLO 

Si hacemos una búsqueda de los permisos SUID, vemos que Python se encuentra entre ellos:
![[Pasted image 20230401015128.png]]
Y también vemos estos permisos para Python:
![[Pasted image 20230401015233.png]]
Por tanto nos creamos un fichero de Python con un código que nos permita escalar privilegios:
![[Pasted image 20230401015723.png]]
Lo compartimos con la máquina víctima:
![[Pasted image 20230401015746.png]]
Y lo ejecutamos para convertirnos en root:
![[Pasted image 20230401015812.png]]
![[Pasted image 20230401015823.png]]
[[MAQUINA ROOTME (Fuzzing, php malicioso camuflándolo como phtml en burpsuite, escalada de privilegios comprobando permisos SUID de Python)]]