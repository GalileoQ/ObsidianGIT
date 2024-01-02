### nmap
```python
Nmap scan report for 10.10.203.121
Host is up (0.23s latency).
Not shown: 65358 closed tcp ports (reset), 175 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b7:4c:d0:bd:e2:7b:1b:15:72:27:64:56:29:15:ea:23 (RSA)
|   256 b7:85:23:11:4f:44:fa:22:00:8e:40:77:5e:cf:28:7c (ECDSA)
|_  256 a9:fe:4b:82:bf:89:34:59:36:5b:ec:da:c2:d3:95:ce (ED25519)
10000/tcp open  http    MiniServ 1.890 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 116.37 seconds
```

### ports: 22/ssh - 1000/http 

### Enumeracion del puerto 10000 http
![[Pasted image 20231130200443.png]]
### parece que la pagina corre por https asi que debemos especificarlo en la url
![[Pasted image 20231130200653.png]]
### damos en advanced - luego en accept the risk and continue 
![[Pasted image 20231130201108.png]]
### tenemos una pagina de login. de la cual no conocemos las credenciales

### Buscaremos un exploit para esta web y encontramos uno que con la bandera -cmd nos permite ejecutar comandos.
![[Pasted image 20231130204614.png]]
### explicacion:

```python
	1) ejecutamos el exploit y usamos whoami para asegurarnos que si funciona. nos indica que simos root
	2) creamos un archivo index.html que contenta una revershell y creamos un servidor en python
	3) ejecutamos el exploit y hacemos un curl a nuestra ip para llamar al archivo
	4) nos ponemos a la escucha en el puerto que hemos especificado
	5) ejecutamos el exploit y hemos ganado una shell
```
### WE ARE ROOT