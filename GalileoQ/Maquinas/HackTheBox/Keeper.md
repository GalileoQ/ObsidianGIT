### nmap
```python
Nmap scan report for 10.10.11.227
Host is up (0.074s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 35:39:d4:39:40:4b:1f:61:86:dd:7c:37:bb:4b:98:9e (ECDSA)
|_  256 1a:e9:72:be:8b:b1:05:d5:ef:fe:dd:80:d8:ef:c0:66 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.57 seconds
```
### Ports: 22/ssh - 80/http

### Enumeracion de puerto 80(http)
![[Pasted image 20231110121713.png]]
la pagina nos da un enlace de re-dirección.
![[Pasted image 20231110121931.png]]
### parece que no accedemos al host por lo que debemos ingresarlo en nuestro archivo de configuracion /etc/hosts
comando :
```python
	sudo nano /etc/hosts
```
![[Pasted image 20231110123450.png]]
en este caso nos quedaria algo asi:
```python
	IP                      Hosts                url hosts de redireccion
	
	10.10.11.227            http://keeper         tickets.keeper.htb 
```
### perfecto. agregamos la url a donde estamos siendo redirigidos. guardamos y recargamos la pagina
![[Pasted image 20231110124609.png]]
### tenemos una pagina de login.
podemos ver la versión de la pagina. podriamos buscar credenciales por defecto y exploits para esta version
![[Pasted image 20231110125120.png]]
### haciendo una busqueda rapida en google tenemos credenciales
```python
	root
	password
```
![[Pasted image 20231110125325.png]]
### estamos dentro de la pagina. vamos a seguir enumerando posibles vías de explotación.
tenemos muchisimas ventanas para enumerar. buscando una por una conseguimos la ventana admin/users
![[Pasted image 20231110131218.png]]
vamos a enumerar al usuario Inorgaard
![[Pasted image 20231110131628.png]]
### tenemos informacion sobre importante sobre este usuario y podemos ver una caja de comentarios donde nos da inofrmacion sobre su contraseña, vamos a intentar usar esta informacion para enumerar el puerto ssh
```python 
	usernam: lnorgaard
	Pass: Welcome2023!
```
tenemos varios archivos interesantes 
![[Pasted image 20231110134928.png]]
### tenemos dos archivos de bases de datos keepass. debemos buscar algun exploit o alguna manera de conseguir la contraseña en estos archivos. 
utilizaremos un exploit para atacar el archivo KeePassDumpFull.dmp 
![[Pasted image 20231110152612.png]]
### hemos conseguido la contraseña medias parece que nos faltan algunos caracteres asi que debemos investigar en google para intentar saber que significa esa convencional de palabras

si limpiamos un poco las lineas y nos quedamos solo con las letras nos queda algo asi:
```python
	●ødgrød , med , fløde
```
vamos a buscar esto en google.
![[Pasted image 20231110153300.png]]
### parece que nuestra contraseña es:
```python
	rødgrød med fløde
```
parece que la letra que faltaba es `r` vamos a probar esta clave para nuestra la base de datos
abrimos keepass2 abrimos nuestro archivo passcode.kdbx
![[Pasted image 20231110155020.png]]
### vamos a mirar la contraseña de root ya que la del usuario lnorgaard ya la tenemos. 
hacemos doble click en root
![[Pasted image 20231110155305.png]]
### passwd:F4><3K0nd!
### hemos probado la contraseña para root pero no funciona. tampoco para ssh asi que podemos mirar esa seccion de notas a ver que podemos sacar de ahi

### despues de una larga busqueda por internet parace que existe una herramienta llamada puttygen que nos permite convertir nuestra PuTTY-User-key-file-3: ssh-rsa a una id_rsa 
para ello debemos instalar puttygen
```python
	sudo apt install putty-tools
```
![[Pasted image 20231110193247.png]]
