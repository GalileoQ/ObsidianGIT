### nmap
```python
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.203.6
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-rw-r--    1 1000     1000       208838 Sep 30  2020 gum_room.jpg
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:31:bb:b5:1f:cc:cc:12:14:8f:f0:d8:33:b0:08:9b (RSA)
|   256 e7:1f:c9:db:3e:aa:44:b6:72:10:3c:ee:db:1d:33:90 (ECDSA)
|_  256 b4:45:02:b6:24:8e:a9:06:5f:6c:79:44:8a:06:55:5e (ED25519)
80/tcp  open  http        Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
100/tcp open  newacct?
| fingerprint-strings: 
|   GenericLines, NULL: 
|     "Welcome to chocolate room!! 
|     ___.---------------.
|     .'__'__'__'__'__,` . ____ ___ \r
|     _:\x20 |:. \x20 ___ \r
|     \'__'__'__'__'_`.__| `. \x20 ___ \r
|     \'__'__'__\x20__'_;-----------------`
|     \|______________________;________________|
|     small hint from Mr.Wonka : Look somewhere else, its not here! ;) 
|_    hope you wont drown Augustus"
```

### Ports : 21/ - 22/  - 80/ - 100/...

### puerto 21 ftp
![[Pasted image 20231206145928.png]]
descargamos el archivo. tenemos una imagen que puede contener informacion oculta
### extraemos informacion oculta de la imagen
![[Pasted image 20231206150844.png]]
nos da un archivo b64.txt que podemos decoder
### tenemos lo que parece ser el archivo /etc/password
![[Pasted image 20231206151351.png]]
### lo decodeamos en base 64 y obtenemos el hash de inicio de sesión de charlie
vamos a decifrar el hash utilizando john
![[Pasted image 20231206152032.png]]

### Puerto 80/http
las credenciales que hemos conseguido nos permiten iniciar session en la pagina web que se encuentra corriendo el el puerto 80 en la cual tenemos una barra que nos permite ejecutar comandos. vamos a aprovechar esto para ejecutar una reverse shell
![[Pasted image 20231206152513.png]]
### reverse shell
buscamos una reverse shell y la ejecutamos desde la linea de comandos de la pagina web
![[Pasted image 20231206152856.png]]
obtenemos conexión con la maquina
### tenemos un archivo bastante sospechoso llamado key_rev_key
![[Pasted image 20231206154332.png]]
### podemos ver una key
ahora vamos al directorio de charlie 
### tenemos un archivo que contiene una id_rsa
![[Pasted image 20231206155455.png]]
### vamos a ver el otro archivo llamado teleport.pub
![[Pasted image 20231206155820.png]]
### Ppuerto 22/ssh vamos a usar la id_rsa que hemos conseguido para iniciar session con el usuario charlie
primero damos permisos de lectura para el archivo id_rsa
![[Pasted image 20231206160323.png]]
![[Pasted image 20231206160625.png]]
somos el usuario charlie
### Escalada de privilegios
![[Pasted image 20231206160725.png]]
### hacemos sudo -l y podemos ver que todos los usuarios pueden ejecutar el binario /usr/bin/vi sin proporcionar contraseñas asi que vamos a aprovechar esto
![[Pasted image 20231206161304.png]]

### ahora buscamos la flag de root
![[Pasted image 20231206162307.png]]
la clave esta encriptada en un pequeño codigo de python que al ejecutarlo nos pide la primera key que hemos conseguido y nos da la flag de root
### WE ARE ROOT
