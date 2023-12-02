### nmap
```python
Nmap scan report for 10.10.251.27
Host is up (0.22s latency).
Not shown: 65398 closed tcp ports (reset), 135 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 34:0e:fe:06:12:67:3e:a4:eb:ab:7a:c4:81:6d:fe:a9 (RSA)
|   256 49:61:1e:f4:52:6e:7b:29:98:db:30:2d:16:ed:f4:8b (ECDSA)
|_  256 b8:60:c4:5b:b7:b2:d0:23:a0:c7:56:59:5c:63:1e:c4 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: House of danak
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 103.74 seconds
```

### Ports: 22/ssh - 80/http

### Fuzzing con gobuster
![[Pasted image 20231201211706.png]]
tenemos dos directorios. vamos a mirar (mirando el codigo principal de la pag podemos conseguir un usuario llamado john)

### uploads
![[Pasted image 20231201212236.png]]

### secret
![[Pasted image 20231201211847.png]]
###  lo que hemos encontrado es una id_rsa
![[Pasted image 20231201212110.png]]
### la id_rsa esta encriptada asi que la desencriptamos con con ssh2john
![[Pasted image 20231201213400.png]]

### usaremos estas credenciales para ingresar via ssh

```python
ssh -i id_rsa -p 22 john@IP
```

### escalada de privilegios
el usuario pertenece a un grupo llamado (lxd) despues de hacer una busqueda esto parece ser un 