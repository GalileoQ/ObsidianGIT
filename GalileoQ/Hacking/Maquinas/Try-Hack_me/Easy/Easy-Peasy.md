#Linux #Easy 
### nmap 
```python
PORT      STATE SERVICE VERSION
80/tcp    open  http    nginx 1.16.1
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: nginx/1.16.1
|_http-title: Welcome to nginx!
6498/tcp  open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 30:4a:2b:22:ac:d9:56:09:f2:da:12:20:57:f4:6c:d4 (RSA)
|   256 bf:86:c9:c7:b7:ef:8c:8b:b9:94:ae:01:88:c0:85:4d (ECDSA)
|_  256 a1:72:ef:6c:81:29:13:ef:5a:6c:24:03:4c:fe:3d:0b (ED25519)
65524/tcp open  http    Apache httpd 2.4.43 ((Ubuntu))
|_http-server-header: Apache/2.4.43 (Ubuntu)
|_http-title: Apache2 Debian Default Page: It works
| http-robots.txt: 1 disallowed entry 
|_/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 91.73 seconds
```
### Ports: 80/http - 6498/ssh - 65524/http

### Enumeracion del puerto 80/65524
![[Pasted image 20240612143517.png]]

![[Pasted image 20240612143521.png]]

### en el codigo fuente de la maquina haciendo redireccion al puerto 65524 tenemos algo oculto
![[Pasted image 20240612143526.png]]
### vamos a desencriptar este codigo el hash analizer nos indica que esta en base62 asi que vamos a decodear
![[Pasted image 20240612143536.png]]
### encontramos lo que parece ser un sub directorio,  investigamos y tenemos mas informacion
vamos a descargar la imagen
![[Pasted image 20240612143543.png]]
### tenemos otro codigo que podeos decencriptar 
![[Pasted image 20240612143547.png]]
### extraemos la informacion oculta en la imagen y utilizamos esta password 
![[Pasted image 20240612143551.png]]
### este archivo nos entrega el usuario boring un codigo binario
![[Pasted image 20240612143554.png]]
### nuestro escaneo de nmap nos dice que en el puerto 65524 esta un recurso compartido llamado robots.txt
aca tenemos la primera flag
![[Pasted image 20240612143559.png]]
### conseguimos la flag 1 en los dos directorios que hemos encontrado con gobuster
solo tenemos que decodearla
![[Pasted image 20240612143604.png]]

### conseguimos la flag 2 en el archivo robots que se esta compartiendo en el puerto 65524
tenemos que decodearla
![[Pasted image 20240612143610.png]]

### conseguimos la flag 3 en el codigo fuente del puerto 65524
tenemos que decodear
![[Pasted image 20240612143620.png]]
### Enumeracion del puerto 6498/ssh
![[Pasted image 20240612143624.png]]
### Escalada de privilegios
![[Pasted image 20240612143646.png]]
### tenemos una tarea crontab en la que tenemos permisos de ejecusion
![[Pasted image 20240612143651.png]]
hacemos ls y tenemos permisos para editar esta tarea crontab 
### editamos los permisos de la bash
![[Pasted image 20240612143657.png]]
### ejecutamos bash -p 
![[Pasted image 20240612143700.png]]
### WE ARE ROOT
