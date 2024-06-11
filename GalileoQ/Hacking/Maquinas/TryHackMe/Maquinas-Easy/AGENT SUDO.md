#Linux #Easy
Haremos el reconocimiento con nmap:
![[Pasted image 20230528202316.png]]
Si miramos la web, sería esta:
![[Pasted image 20230528202426.png]]
Vamos a probar en interceptar esta petición con burpsuite e intentar manipularla cambiando el usuario, donde si vemos que nos habla el usuario R dentro de la web, podemos probar en poner otro usuario con burpsuite, por ejemplo el user-agent C:
![[Pasted image 20230528203226.png]]
Y al enviar esta petición vemos que se aplica un redirect donde nos encontramos con el usuario chris:
![[Pasted image 20230528203259.png]]
Una vez que sabemos el usuario chris, podemos hacer un ataque de fuerza bruta contra el puerto ftp, el puerto 21: [[1 - Hydra - Ataque de Fuerza Bruta FTP]]
![[Pasted image 20230528204757.png]]
Y si entramos por FTP nos encontramos con varias fotos y un documento txt, que puede ser interesante para aplicar esteganografía en busca de algo oculto en estos archivos:
![[Pasted image 20230601213839.png]]
Y si usamos el comando mget * nos bajamos todos los archivos del presente directorio:
![[Pasted image 20230601214009.png]]
Y ahora con el comando binwalk podemos buscar si dentro de estas imágenes hay algo, y vemos que sí:
![[Pasted image 20230601214121.png]]
Y para extraer un archivo zip de una foto podemos usar una herramienta llamada foremost; y podemos usarla de esta forma:
![[Pasted image 20230601214703.png]]
Y dentro de la carpeta zip tenemos un archivo comprimido, que si lo crackeamos con john the ripper podremos obtener su contraseña:
![[Pasted image 20230601214845.png]]
Ahora para descomprimir este archivo .zip pasándole la contraseña, tenemos que usar 7z e 00000067.zip:
![[Pasted image 20230601220357.png]]
Y este fichero de texto tiene un mensaje o una posible contraseña:
![[Pasted image 20230601220428.png]]
Y si lo decodificamos con base64 veremos que nos saca la contraseña Area51:
![[Pasted image 20230601220501.png]]
Y ahora con esta contraseña vamos a intentar extraer algo en la imagen cute-alien.jpg; y vemos que tenemos un fichero llamado message.txt:
![[Pasted image 20230601220802.png]]
![[Pasted image 20230601220813.png]]
Y ahí tenemos un usuario james y una contraseña hackerrules!
![[Pasted image 20230601220928.png]]
Podemos ejecutar Linux exploit suggester y encontraremos varios exploits que quizá funcionen, aunque podemos mirar la versión de sudo con el comando sudo -V, la cual es vulnerable, y ademas vemos que el linux exploit suggester también nos recomienda esa vulnerabilidad pública:
![[Pasted image 20230605105011.png]]
![[Pasted image 20230605104655.png]]
Buscamos un exploit por github y encontramos este:
![[Pasted image 20230605105033.png]]
Y nos dice que primero probemos en ejecutar el exploit_nss.py:
![[Pasted image 20230605105100.png]]
Por tanto nos lo compartimos con la máquina víctima, lo ejeutamos y ya somos root:
![[Pasted image 20230605105137.png]]
![[Pasted image 20230605105144.png]]
