### nmap
```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-08 20:49 EST
Nmap scan report for 10.10.11.252
Host is up (0.073s latency).
Not shown: 62002 closed tcp ports (reset), 3530 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 3e:21:d5:dc:2e:61:eb:8f:a6:3b:24:2a:b7:1c:05:d3 (RSA)
|   256 39:11:42:3f:0c:25:00:08:d7:2f:1b:51:e0:43:9d:85 (ECDSA)
|_  256 b0:6f:a0:0a:9e:df:b1:7a:49:78:86:b2:35:40:ec:95 (ED25519)
80/tcp  open  http     nginx 1.18.0
|_http-server-header: nginx/1.18.0
|_http-title: Did not follow redirect to https://bizness.htb/
443/tcp open  ssl/http nginx 1.18.0
| tls-alpn: 
|_  http/1.1
| tls-nextprotoneg: 
|_  http/1.1
| ssl-cert: Subject: organizationName=Internet Widgits Pty Ltd/stateOrProvinceName=Some-State/countryName=UK
| Not valid before: 2023-12-14T20:03:40
|_Not valid after:  2328-11-10T20:03:40
|_http-title: Did not follow redirect to https://bizness.htb/
|_http-server-header: nginx/1.18.0
|_ssl-date: TLS randomness does not represent time
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 53.45 seconds
```

### Ports: 22/ssh - 80/http - 443/ssl

### Enumeracion del puerto 80
![[Pasted image 20240108215937.png]]
tenemos los apartados de home/about us/services/contact. debemos hacer una recopilacion pasiva de informacion
```python
Informacion

Address: - A108 Adam Street, NY 535022, USA
Phone: - [+1 5589 55488 55](tel:+155895548855)
Email: - info@bizness.htb
```
### Fuzzing con dirb
![[Pasted image 20240108221622.png]]
en este caso usamos la herramienta dirb y conseguimos diferentes subdirectorios. en el directorio accounting tenemos un panel de login
![[Pasted image 20240108221731.png]]


### despues de una larga busqueda en internet tenemos un CVE posiblemente funcional asi que vamos a probar 
primero tenemos que inetalar los requerimentos 
![[Pasted image 20240108225217.png]]
una vez tenemos todos los requerimentos podemos ver como se instala y se ejecuta
![[Pasted image 20240108225711.png]]
este exploit hace que la pag sea vulnerable y ahora si podemos atacarla

### una ves nuestra maquina es vulnerable podemos intentar copiar el binario de netcat para ponernos a la escucha
```python
cp /usr/bin/nc ./
```
### ejecutamos el exploits y nos ponemos a la escucha en el puerto correspondiente y estaremos dentro

![[Pasted image 20240130202648.png]]
### WE ARE ROOT
