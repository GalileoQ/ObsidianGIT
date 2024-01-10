```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 74:e0:e1:b4:05:85:6a:15:68:7e:16:da:f2:c7:6b:ee (RSA)
|   256 bd:43:62:b9:a1:86:51:36:f8:c7:df:f9:0f:63:8f:a3 (ECDSA)
|_  256 f9:e7:da:07:8f:10:af:97:0b:32:87:c9:32:d7:1b:76 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Smag
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 84.71 seconds
```

### Ports: 22/ssh - 80/http

### Enumeracion del puerto 80
![[Pasted image 20240110174447.png]]
parece que no tenemos mucha información debido a que la pagina esta en mantenimiento. y mirando el código fuente de la pagina tampoco podemos conseguir mucha información

### Fuzzing con gobuster
![[Pasted image 20240110175151.png]]
temos un subdominio llamado /mail asi que vamos a investigar 

### subdominio /mail
tenemos tres mensajes en esta dirección que nos proporcionan información valiosa
![[Pasted image 20240110175916.png]]
tenemos un archivo para descargar y un usuario llamado Uzi