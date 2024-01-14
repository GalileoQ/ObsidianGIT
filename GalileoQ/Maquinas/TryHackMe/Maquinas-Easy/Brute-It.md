### nmap
```python
sudo nmap -p- --open -sS -sCV -n --min-rate 2500 -Pn 10.10.41.104
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-12 23:46 EST
Nmap scan report for 10.10.41.104
Host is up (0.24s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4b:0e:bf:14:fa:54:b3:5c:44:15:ed:b2:5d:a0:ac:8f (RSA)
|   256 d0:3a:81:55:13:5e:87:0c:e8:52:1e:cf:44:e0:3a:54 (ECDSA)
|_  256 da:ce:79:e0:45:eb:17:25:ef:62:ac:98:f0:cf:bb:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 58.71 seconds
```
### ports: 22-ssh/80-http
### enumeracion de puerto 80-http/pagina-web
![[Pasted image 20231113005140.png]]
### aplicamos fuzzing con gobuster.
![[Pasted image 20231113005700.png]]
tenemos un directorio llamado /admin
![[Pasted image 20231113005820.png]]
miramos el código fuente de la pagina
![[Pasted image 20231113005926.png]]
nos dicen que el username de la pagina es admin
### vamos hacer fuzzing sobre el directorio /admin
![[Pasted image 20231113012255.png]]
### al parecer nos hace una mala redireccion asi que vamos a interceptar con BurpSuite
![[Pasted image 20231113013055.png]]
### tenemos un usuario llamado john y tambien tenemos una id_rsa la cual buscaremos en la pagina web
![[Pasted image 20231113013354.png]]
### pasaremos esa id_rsa por ssh2john para decodearla y luego usaremos john para conseguir el passwd
![[Pasted image 20231113022629.png]]
### vamos a aplicar fuerza bruta al panel de login
![[Pasted image 20231113030834.png]]
### Credenciales
```python
	Username: admin
	Password: xavier
```
### vamos a entrar por ssh con las credenciales de john. tenemos el id_rsa y la clave de ese id_rsa que seria rockinroll
![[Pasted image 20231113032241.png]]
### podemos ver que tenemos permisos de root y sin usar contraseñas podemos usar el binario /bin/cat y usaremos este binario para hacer un cat a los shadow del sistema
![[Pasted image 20231113032937.png]]
### tenemos un shadow para el usuario root asi que lo copiamos y lo vamos a crackear con john
![[Pasted image 20231113033200.png]]
### cambiamos de usuario con su root
![[Pasted image 20231113033453.png]]
### ya somos root
