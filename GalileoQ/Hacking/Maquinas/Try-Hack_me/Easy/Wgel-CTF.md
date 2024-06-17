#Linux #Easy 
### nmap
```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 94:96:1b:66:80:1b:76:48:68:2d:14:b5:9a:01:aa:aa (RSA)
|   256 18:f7:10:cc:5f:40:f6:cf:92:f8:69:16:e2:48:f4:38 (ECDSA)
|_  256 b9:0b:97:2e:45:9b:f3:2a:4b:11:c7:83:10:33:e0:ce (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 80.24 seconds
```
##### Ports: 22/ssh - 80/http
### haremos fuzzing usando la herramienta gobuster
![[Pasted image 20240612151214.png]]
### tenemos varios sub-directorios. 
miramos en /sitemap
![[Pasted image 20240612151221.png]]
### parece que no tenemos nada interesante mas que la la pestaña de contacto con informacion
![[Pasted image 20240612151225.png]]
### vamos hacer fuzzing al directorio que hemos conseguido /sitemap
![[Pasted image 20240612151229.png]]
### conseguimos un directorio llamado .ssh
vamos a buscar en esa ruta
![[Pasted image 20240612151234.png]]
tenemos una id_rsa
### vamos a crackear esa id_rsa
![[Pasted image 20240612151237.png]]
### la id_rsa no tiene password asi que esta lista para ser usada. pero nos falta un usuario asi que vamos a buscarlo

### mirando el codigo fuente de todas las paginas hemos conseguido enumerar un usuario
![[Pasted image 20240612151241.png]]
### le daremos permisos de lectura a la id_rsa y juego vamos a conectarnos con la información que hemos conseguido
![[Pasted image 20240612151244.png]]
#### tenemos conexión
### navegamos por los diferentes directorios hasta conseguir la user_flag.txt
![[Pasted image 20240612151248.png]]
### enumeramos permisos sudo para el usuario jessie
![[Pasted image 20240612151253.png]]
### usando el comando wget hacemos un post de /etc/shadow sobre nuestra maquina atacante
![[Pasted image 20240612151259.png]]
### copiamos este archivo shadow en nuestra maquina y usaremos el comando openssl para crear una clave para el usuario root
![[Pasted image 20240612151305.png]]

```python
openssl passwd: Este es el comando de OpenSSL para generar contraseñas hash.

-6: Especifica el uso del algoritmo de hash SHA-512. En sistemas basados en Unix.

-salt 'salt': Define el salt utilizada en la generación del hash.
    
'password': Es la contraseña que deseamos convertir en un hash.
```

### vamos a reemplazar este hash en el shadow para asignarla al usuario root
![[Pasted image 20240612151310.png]]
### ahora vamos a enviar el nuevo shadow con la contraseña que hemos creado para root a la maquina victima utilizando wget
creamos un servidor en python en nuestra maquina atacante y desde la maquina victima hacemos una solicitud con wget y con el parametro -O le indicamos que nos guarde el output en la direccion /etc/shadow. de esta manera reescribiremos el archivo shadow con el nuevo que hemos enviado el cual tiene la clave que nosotros elegimos anteriormente
![[Pasted image 20240612151316.png]]
### ahora solo vamos a cambiar de usuario y usaremos la calve que hemos definido para root
![[Pasted image 20240612151320.png]]
### WE ARE ROOT.
