### nmap 
```python
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 de:5b:0e:b5:40:aa:43:4d:2a:83:31:14:20:77:9c:a1 (RSA)
|   256 f4:b5:a6:60:f4:d1:bf:e2:85:2e:2e:7e:5f:4c:ce:38 (ECDSA)
|_  256 29:e6:61:09:ed:8a:88:2b:55:74:f2:b7:33:ae:df:c8 (ED25519)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
|_http-title: Overpass Hosting
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.37 (centos)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.86 seconds
```
### Ports: 21/ftp - 22/ssh - 80/http

### Enumeracion del puerto 80/http

![[Pasted image 20240128163523.png]]
tenemos un archivo llamado backups

### tenemos un archivo pgp
vamos a agregar la clave privada al archivo 
![[Pasted image 20240128163757.png]]

### pgp decrypt
![[Pasted image 20240128164456.png]]
hemos guardado la salida en un nuevo archivo llamado CustomerDetails.xlsx

### miramos el archivo con una herramienta online
![[Pasted image 20240128164700.png]]
tenemos 3 usuarios y 3 password

### f