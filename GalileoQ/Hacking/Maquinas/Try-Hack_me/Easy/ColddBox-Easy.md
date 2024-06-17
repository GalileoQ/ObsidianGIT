#Linux #Easy 
### nmap
```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-06 11:52 EST
Nmap scan report for 10.10.35.246
Host is up (0.21s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-generator: WordPress 4.1.31
|_http-title: ColddBox | One more machine
|_http-server-header: Apache/2.4.18 (Ubuntu)
4512/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4e:bf:98:c0:9b:c5:36:80:8c:96:e8:96:95:65:97:3b (RSA)
|   256 88:17:f1:a8:44:f7:f8:06:2f:d3:4f:73:32:98:c7:c5 (ECDSA)
|_  256 f2:fc:6c:75:08:20:b1:b2:51:2d:94:d6:94:d7:51:4f (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 75.77 seconds
```
### Enumeracion del puerto 80
![[Pasted image 20240612142643.png]]

### utilizaremos la eramienta wpscan
![[Pasted image 20240612142647.png]]
conseguimos uun posible archivo XML
### usuarios
![[Pasted image 20240612142652.png]]
tenemos al usuario philip, c0ldd, hugo. creamos un pequeño diccionario con estos usuarios y lo usaremos para hacer fuerza bruta
![[Pasted image 20240612142657.png]]

![[Pasted image 20240612142700.png]]
### aqui estamos haciendo un ataque de fueraza bruta para conseguir la contraseña de alguno de los usuarios 
Conseguimos la contraseña
![[Pasted image 20240612142706.png]]
### Credenciales
```python
users: c0ldd
passw: 9876543210
```
### ingresamos al wordpress
![[Pasted image 20240612142710.png]]
### vamos al apartado de themas y podemos editar un thema 
![[Pasted image 20240612142716.png]]
aqui vamos a inyectar comando en php
![[Pasted image 20240612142720.png]]
### inyectamos nuestro codigo php y buscamos la ruta exacta donde se guarda esta template
![[Pasted image 20240612142725.png]]
ya estamos dentro
### En la ruta /var/www/html podemos conseguir un archivo de configuracion que contiene credenciales
![[Pasted image 20240612142729.png]]
### Escalada de privilegios 
![[Pasted image 20240612142733.png]]
el usuario c0ldd puede ejecutar los siguientes comandos como root....
usaremos el comando chmod para cambiar los permisos de la bash y asi poder spaunear una bash como root
![[Pasted image 20240612142737.png]]
### WE ARE ROOT