Vamos a ver la versión de ubuntu que estamos utilizando, y vemos que es la 12.04, para ello podemos usar lsb_release -a:
![[Pasted image 20240612155815.png]]
Podemos buscar por internet algún exploit para esta versión de ubuntu, y nos encontramos con un exploit de la web de Exploit-DB:
![[Pasted image 20240612155822.png]]

![[Pasted image 20240612155825.png]]
Nos lo bajamos y lo compartimos con la máquina víctima:
![[Pasted image 20240612155835.png]]

![[Pasted image 20240612155839.png]]

![[Pasted image 20240612155843.png]]

Ahora que ya lo tenemos dentro de la máquina víctima, simplemente tenemos que compilarlo para poder ejecutarlo; y por tanto utilizamos el comando gcc -o escalada 37292.c:
![[Pasted image 20240612155847.png]]

Y ahora sólo tenemos que ejecutar el script de ./escalada y ya somos usuarios root:
![[Pasted image 20240612155857.png]]

Y aquí tenemos la flag de root:
![[Pasted image 20240612155911.png]]
![[Pasted image 20240612155914.png]]

------------------------------------

[[MAQUINA CYBERSPLOIT (Decodificar base64, Inspeccionar código fuente de la página, convertir archivo en binario, escalada de privilegios en Ubuntu 12.04 con exploit de exploitdb overlayfs y compilar exploit en .c con el comando gcc)]]