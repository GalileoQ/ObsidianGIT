### Ping
```python
ping -c 1 10.10.10.175
PING 10.10.10.175 (10.10.10.175) 56(84) bytes of data.
64 bytes from 10.10.10.175: icmp_seq=1 ttl=127 time=152 ms

--- 10.10.10.175 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 152.043/152.043/152.043/0.000 ms
```

### TTL 127=Windows

### nmap
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Egotistical Bank :: Home
| http-methods: 
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-03-13 08:38:06Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49676/tcp open  msrpc         Microsoft Windows RPC
49677/tcp open  msrpc         Microsoft Windows RPC
49736/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: SAUNA; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 6h59m08s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-03-13T08:38:57
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 140.36 seconds
```
podemos enumerar un domain controller  para el dominio EGOTISTICAL-BANK.LOCAL y LDAP corriendo por el puerto 80 y el 389 respectivamente

### Enumeración del puerto 445 (SMB)
utilizamos diferentes herramientas para intentar enumerar el puerto SMB pero no tenemos exito
![[Pasted image 20240312225027.png]]

### Enumeración del Domain Controller
intentaremos enumerar el domain controller usando rpcclinet pero tampoco tenemos exito
![[Pasted image 20240312225457.png]]

### Enumeración del puerto 389 (LDAP)
usaremos la herramienta [windapsearch](https://github.com/ropnop/windapsearch) para enumerar el puerto LDAP. en este caso no obtenemos nada
![[Pasted image 20240312233139.png]]

### impacket-GetADUsers
usamos la herramienta de impacket pero tampoco tenemos éxito así que seguiremos enumerando
![[Pasted image 20240312233348.png]]

### Fuzzing con ffuz

![[Pasted image 20240312235017.png]]

### Enumeración del puerto 80 (http)
vamos al directorio About Us y hacemos scroll hacia abajo 
![[Pasted image 20240312234526.png]]

### About Us
aqui podemos ver los nombres completos de algunos empleados del banco
![[Pasted image 20240312234728.png]]

### username-anarchy
vamos a usar este herramienta para hacer permutaciones entre los usuarios que hemos conseguido
![[Pasted image 20240312235201.png]]

creamos una lista de usuarios en un archivo txt
![[Pasted image 20240313003316.png]]

ahora vamos hacer algunas permutaciones en los nombres para tener muchas mas variantes
```python
# ejemplo
nombre apellido
nombre.apellido
nombre
apellido
nombrea    
napellido
```

![[Pasted image 20240313004436.png]]

### kerbrute
podemos descargar kerbruter desde [kerbrute](https://github.com/ropnop/kerbrute) 
```python 
cd kerbrute
go buil .
```
liste de esta manera ya tenemos kerbrute preparado
![[Pasted image 20240313000615.png]]

ejecutamos kerbrute
logramos conseguir un usuario valido **fsmith**
![[Pasted image 20240313005637.png]]

### impacket-GetNPUsers
El módulo `GetNPUsers` de Impacket nos permite extraer información sobre cuentas de usuario que tienen configurada la propiedad `Do Not Require Preauthentication` (no requerir autenticación previa) en un dominio de Active Directory. Esta característica de seguridad cuando está habilitada para una cuenta de usuario en particular, permite que un atacante pueda realizar ataques de captura de hash de autenticación (ASREPRoasting) contra esa cuenta.
![[Pasted image 20240313010322.png]]
logramos obtener un TGT `Ticket Granting Ticket` esto es un hash. para el usuario fsmith vamos a intentar romperlo con hashcat

### hashcat
```python
hashcat --example-hashes # podemos ver un ejemplo de todos los hashes que estan en hashcat

hashcat --example-hashes | grep krb5asrep # `krb5asrep` pertenece a el primer parametro del hash. suelen estan encerrados por el sombilo $

hashcat --example-hashes | grep krb5asrep -B 20 # -B 20 nos permite capturar las primeras 20 lineas del resultado de identificar el hash
```

con estas opciones logramos obtener el tipo de hash al que nos estamos enfrentando en este caso es `HAsh mode #18200`
![[Pasted image 20240313200051.png]]

Iniciando 
![[Pasted image 20240313201029.png]]

de esta manera logramos obtener la contraseña perteneciente a este hash
![[Pasted image 20240313201114.png]]

### crackmapexec
podemos usar crackmapexec para intentar validar estas credenciales. si vemos un `+` esto nos indica que las credenciales son validas. una vez confirmado esto también podemos usar crackmapexec winrm para saber si el usuario `fsmith` forma parte del grupo `Remote Management Users`
![[Pasted image 20240313203544.png]]

### evil-winrm

![[Pasted image 20240313204137.png]]

