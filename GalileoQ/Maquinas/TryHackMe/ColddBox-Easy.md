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
![[Pasted image 20240106131915.png]]

### utilizaremos la eramienta wpscan
![[Pasted image 20240106132000.png]]
conseguimos uun posible archivo XML
### usuarios
![[Pasted image 20240106132420.png]]
tenemos al usuario philip, c0ldd, hugo. creamos un pequeño diccionario con estos usuarios y lo usaremos para hacer fuerza bruta
![[Pasted image 20240106132913.png]]
![[Pasted image 20240106133114.png]]
### aqui estamos haciendo un ataque de fueraza bruta para conseguir la contraseña de alguno de los usuarios 
Conseguimos la contraseña
![[Pasted image 20240106133342.png]]
### Credenciales
```python
users: c0ldd
passw: 9876543210
```
### ingresamos al wordpress
![[Pasted image 20240106133706.png]]
