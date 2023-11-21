### nmap
```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 76:26:67:a6:b0:08:0e:ed:34:58:5b:4e:77:45:92:57 (RSA)
|   256 52:3a:ad:26:7f:6e:3f:23:f9:e4:ef:e8:5a:c8:42:5c (ECDSA)
|_  256 71:df:6e:81:f0:80:79:71:a8:da:2e:1e:56:c4:de:bb (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 89.87 seconds
```
### ports: 22/ssh - 80/http

### empezamos enumerando el puerto 80
![[Pasted image 20231121122855.png]]
en este caso parece que no tenemos mucho
### Fuzzing con gobuster
![[Pasted image 20231121123528.png]]
### tenemos un directorio llamado app. en el que encontramos una carpeta llamada pluck
![[Pasted image 20231121123646.png]]
### nos lleva a lo que parece ser la pagina principal
![[Pasted image 20231121123850.png]]
### aquí podemos enumerar el framework en el que esta echo la pagina y también un usuario admin
### si entramos en admin nos dirige a una pagina donde podremos logearnos pero aun no tenemos credenciales. lo que si podemos conseguir es lo que parece ser la version del framework y este lo podemos usar para buscar exploit que nos ayuden a entrar en la pagina
![[Pasted image 20231121124556.png]]
### despues de 