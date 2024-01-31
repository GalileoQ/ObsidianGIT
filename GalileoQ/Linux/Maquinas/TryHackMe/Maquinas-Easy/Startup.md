### namp
```python
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.8.203.6
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp [NSE: writeable]
| -rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
|_-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b9:a6:0b:84:1d:22:01:a4:01:30:48:43:61:2b:ab:94 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDAzds8QxN5Q2TsERsJ98huSiuasmToUDi9JYWVegfTMV4Fn7t6/2ENm/9uYblUv+pLBnYeGo3XQGV23foZIIVMlLaC6ulYwuDOxy6KtHauVMlPRvYQd77xSCUqcM1ov9d00Y2y5eb7S6E7zIQCGFhm/jj5ui6bcr6wAIYtfpJ8UXnlHg5f/mJgwwAteQoUtxVgQWPsmfcmWvhreJ0/BF0kZJqi6uJUfOZHoUm4woJ15UYioryT6ZIw/ORL6l/LXy2RlhySNWi6P9y8UXrgKdViIlNCun7Cz80Cfc16za/8cdlthD1czxm4m5hSVwYYQK3C7mDZ0/jung0/AJzl48X1
|   256 ec:13:25:8c:18:20:36:e6:ce:91:0e:16:26:eb:a2:be (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOKJ0cuq3nTYxoHlMcS3xvNisI5sKawbZHhAamhgDZTM989wIUonhYU19Jty5+fUoJKbaPIEBeMmA32XhHy+Y+E=
|   256 a2:ff:2a:72:81:aa:a2:9f:55:a4:dc:92:23:e6:b4:3f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPnFr/4W5WTyh9XBSykso6eSO6tE0Aio3gWM8Zdsckwo
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD POST
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Maintenance
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kerne
```

### con nuestra primera enumeracion de nmap podemos observar que los puertos 80,22 y 21 estan abiertos. el puerto 21 tiene una missconfiguration en la cual se puede ingresar al puerto con las credenciales de anonymous. 

entramos al puerto fpt. cuando nos pide el nombre ponemos anonimos y cuando pide clave le damos enter y la dejamos en blanco
![[2023-11-04_13-45.png]]

una ves dentro hacemos un dir para enumerar los directorios
![[2023-11-04_13-50.png]]
despues podemos hacer un `mget *` para descargar todos los archivos a nuestra maquina

obtenemos esto:
![[2023-11-04_13-55.png]]

### tenemos dos archivos uno .txt y otro es una imagen .jpg. nuestro archivo txt nos dice que existe un usuario llamado maya asi que ya podemos agregar esta informacion a nuestra enumeracion.


### vamos a hacer fuzzing a la pagina web para poder enumerar directorios en busca de algo que nos sea ultil

comando
```python
gobuster dir -u http://10.10.219.247 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

![[Pasted image 20231104140022.png]]

### por el momento podemos observar que existe un direcctorio llamado /files asi que echemos un vistazo

![[Pasted image 20231104140357.png]]

### en este direcctorio tenemos los mismos archivos que hemos conseguido enumerando el puerto ftp ademas podemos ver la carpeta ftp que tambien podemos ver desde el pierto ftp. esto es interesante.

### ok hemos enumerado el puerto 21(ftp) y el puerto 80 (http) nos faltaria enumerar el puerto 22 (ssh) hemos conseguido un usuario llamado maya asi que podemos aplicar fueza bruta sobre ese usuario.

![[Pasted image 20231104142617.png]]

### bueno parece que no podremos hacer fuerza bruta sobre este usuario. vamos a ingresar de nuevo al puerto ftp para echar un vistazo a esa carpeta ftp.

![[Pasted image 20231104143405.png]]

### vale. podemos ingresar en la carpeta ftp que se muestra en nuestro navegado. podriamos intentar subir algun archivo malosioso en esta carpeta de manera que cuando lo llamemos desde la web nos ejecute una reverse shell.

### vamos a buscar [pentestmonkey/php-reverse-shell (github.com)](https://github.com/pentestmonkey/php-reverse-shell) esta pagina nos proporciona un codigo php el cual nos podria ejecutar una reverse shell. solo debemos modificar la ip y el puerto

![[Pasted image 20231104144705.png]]
### aqui debemos poner nuestra ip de atacante y en port usaremos un puerto por donde estaremos en escucha posteriormente.

### para llamar al archivo desde el servicio ftp hacemos un "put + nombre del archivo" despues de esto vamos a la pagina y lo buscamos en la carpeta que se llama ftp y hacemos click para que se ejecute. tendremos una shell.

### somos el usuario www-date asi que probablemente trendremos que hacer movimiento.

![[Pasted image 20231104150954.png]]
con este comando intentaremos enumerar los permisos SUID del usuario actual(no encontramos nada)

### buscamos en la carpeta raiz( / ) del sistema 
![[Pasted image 20231104173752.png]]
tenemos una carpeta llamada incidents
![[Pasted image 20231104173909.png]]
dentro tenemos un archivo .pcapng. vamos a descargarnos este archivo montando un servidor en python de la siguiente manera:

```python
	-maquina victima-
	
	python3 -m http.server 9090
	
	-maquina atacante-
	
	nc -lvnp 9090
```

### los archivos .pcapng son archivos de trazado de paquetes. en este caso podemos abrirlo con wireshark para mirar las trazas de paquetes y de esa manera inpeccionar las trazas en busca de informacion

abrimos wireshak vamos a file > open> y seleccionamos nuestro archivo
![[Pasted image 20231104175243.png]]

### en este caso las trazas de paquetes desde el numero 39 al 45 parecen responder a una peticion que se ha echo asi que vamos a inpeccionarlos

podemos dar click derecho > follow > TCP Stream
![[Pasted image 20231104175856.png]]

### parece que alguien a entrado a esta maquina y ha intentado una contraseña con el usuario www-data pero es incorrecta. en la carpeta /home tenemos el usuario lennie. podriamos intentar esta contraseña para ese usuario.

perfecto ha funcionano. ahora somos el usuario lennie
![[Pasted image 20231104180303.png]]

### ahora podemos escalar privilegios para conseguir usuario root.

navegando con el usuario lennie podemos conseguir 2 archivos y una carpeta llamada Documents
![[Pasted image 20231104180901.png]]
dentro de Documents
![[Pasted image 20231104181018.png]]

### vale primero miraremos en el direcctorio scripts porque realmente es raro tener eso asi que podria ayudarnos a escalar privilegios

![[Pasted image 20231104181906.png]]
vamos a ver los permisos de estos archuvos y tambien vamos hacer un cat de planner.sh
![[Pasted image 20231104182219.png]]
parece que el archivo planner.sh esta en la ruta /etc/print.sh y pertenece al usuario root pero el usuario lennie tiene permisos de ejecucion. asi que vamos a editar este archivo para poder conseguir una bash como el dueño del archivo. en este caso root

![[Pasted image 20231104183858.png]]
en este caso usaremos el oneliner mas basico para probar si nos da alguna informacion. nos pondremos en escucha en el puerto 4444 y ejecutaremos el planner.sh

### al ejecutar el archivo planner.sh nos envia una shell como lannie. esto me hace pensar que esto es una tarea crontab. lennie no tiene ninguna tarea crontab asi que debe ser una tarea crontab del usuario root. vamos a esperar un par de minutos

![[Pasted image 20231104191240.png]]
### crontab -l para ver las tareas cron. genial teniamos razon es una tarea cron sobre el usuario root. la shell que nos ha llegado es del usuario root.