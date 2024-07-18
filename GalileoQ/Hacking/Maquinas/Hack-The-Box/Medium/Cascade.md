#ActiveDirectory #medium #windows 
### Ping

```python
ping -c 1 10.10.10.182
PING 10.10.10.182 (10.10.10.182) 56(84) bytes of data.
64 bytes from 10.10.10.182: icmp_seq=1 ttl=127 time=156 ms

--- 10.10.10.182 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 156.382/156.382/156.382/0.000 ms
```

### TTL = 127 = Windows

### nmap

```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-17 20:45 EDT
Nmap scan report for 10.10.10.182
Host is up (0.16s latency).
Not shown: 65520 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-07-18 00:46:16Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: cascade.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: cascade.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         Microsoft Windows RPC
49170/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: CASC-DC1; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-07-18T00:47:08
|_  start_date: 2024-07-17T14:29:41

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 127.70 seconds
```

### Enumeración del puerto 445
usando la herramienta smbclient no podemos enumerar ya que no tenemos credenciales
![[Pasted image 20240717210744.png]]

### crackmapexec smb
con esta herramienta podemos enumerar usuarios con una lista de usuario proporcionadas por la herramienta
![[Pasted image 20240717211832.png]]


### Opción 2 nxc
con esta herramienta podemos realizar el mismo escaneo y obtenemos la misma información
![[Pasted image 20240717212411.png]]

### rpcclient
de esta forma obtenemos la misma lista de usuarios así que ya podemos guardarnos estos usuarios para hacer mas pruebas adelante
![[Pasted image 20240717210550.png]]

`guardamos los usuarios en un archivo para usarlos mas adelante`
![[Pasted image 20240717211348.png]]

### ldapsearch 
de esta manera podemos ver que el usuario `r.thompson` esta pwn así que solo debemos capturar estas credenciales
![[Pasted image 20240717214515.png]]

`base64 -d`

![[Pasted image 20240717215410.png]]

### evil-winrm
intentamos obtener una conexión pero no fue exitosa así que seguiremos enumerando 
![[Pasted image 20240717215303.png]]


### smbmap
con estas credenciales podemos enumerar el servicio `smb` y listar lis recursos compartidos
![[Pasted image 20240717215735.png]]

### smbmap
dentro de data tenemos cosas interesantes
![[Pasted image 20240717215702.png]]

### smbmap
dentro de los recursos compartidos podemos encontrar un archivo llamado Meeting_Note_June_2018.html así que lo descargamos
![[Pasted image 20240717220206.png]]

qui podemos descargar un segundo archivo
![[Pasted image 20240717221124.png]]

### html
haciendo un servidor en python lo podemos ver desde la web en la cual nos da pistas de lo que esta pasando. tenemos un usuario llamado `TempAdmin`
![[Pasted image 20240717220640.png]]

`VNC\ Install.reg`
en este archivo podemos ver una contraseña que esta en hexadecimal. pero no esta en texto claro así que vamos a investigar para decodearla 
![[Pasted image 20240717221920.png]]

### kerbrute
enumerando con kerbrute podemos verificar todos los usuarios así que vamos a buscar alguno que sea `ASRepRoast` 
![[Pasted image 20240717222809.png]]

### impacket-GetNPUsers
al parecer no podemos obtener un `ASREPROAST` de ningún usuario ya que no cuentan con esta opción seteada 
![[Pasted image 20240717223533.png]]

### ldapdomaindump.py 
usando esta herramienta u usando las credenciales que hemos usado conseguido anteriormente podemos dumpear todo el ldapdomain para poderlo leer de una forma mas cómoda en local
![[Pasted image 20240717225719.png]]

### Domain Users
con un servidor de python podemos compartir los archivos que hemos dumpearlos para poder verlos desde la web
![[Pasted image 20240717230709.png]]

### impacket-GetUserSPNs
tenemos credenciales pero no podemos obtener ningún ataca por el momento 
![[Pasted image 20240717231227.png]]

### vnc decrypte
vamos a seguir con la contraseña que habíamos conseguido en hexadecimal después de investigar un poco en internet podemos conseguir una repo en github que nos proporciona una herramienta útil [vnc-decrypte](https://github.com/jeroennijhof/vncpwd)  dado que la herramienta espera un archivo debemos decodear la password en hexadecimal para guardarlo en un archivo y finalmente pasarlo a la herramienta 

```python
echo "6bcf2a4b6e5aca0f" | xxd -ps -r > passwd

./vncpwd passwd
```

![[Pasted image 20240717233341.png]]

### crackmapexec
hacemos una enumeración con la password que hemos conseguido usando todos los usuarios que ya tenemos para ver si alguno coincide y efectivamente tenemos al usuario `s.smith` 
![[Pasted image 20240717234100.png]]

### Remote Management Users
si verificamos los archivos que hemos dumpeado del ldap podemos ver que el usuario `s.smith` forma parte de este grupo por lo cual podemos intentar conectarnos por `evil-winrm`
![[Pasted image 20240717234442.png]]

### evil-winrm
con estas credenciales y después de asegurarnos que el usuario `s.smith` pertenece al grupo `Remote Management Users` logramos la intrusión 
![[Pasted image 20240718114434.png]]

### Escalada de privilegios
podemos identificar que el usuario `s.smith` pertenece a los grupos `IT` - `Audit Share` 
![[Pasted image 20240718114843.png]]

`localgroup`
enumerando el grupo `Audit Share` podemos ver un comentario. esto es curioso así que vamos a investigar. pero debemos hacerlo enumerando los recursos compartidos desde el usuario `s.smith` ya que desde esta conexión no podemos
![[Pasted image 20240718115356.png]]

