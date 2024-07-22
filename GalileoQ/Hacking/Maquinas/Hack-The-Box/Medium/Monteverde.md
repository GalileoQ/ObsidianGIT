#ActiveDirectory #medium #windows #tecnicas 
### Ping

```python
ping -c 1 10.10.10.172
PING 10.10.10.172 (10.10.10.172) 56(84) bytes of data.
64 bytes from 10.10.10.172: icmp_seq=1 ttl=127 time=272 ms

--- 10.10.10.172 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 271.658/271.658/271.658/0.000 ms
```

### TTL = 127 Windows

### nmap

```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-22 10:01 EDT
Nmap scan report for 10.10.10.172
Host is up (0.15s latency).
Not shown: 65517 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-07-22 14:02:43Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: MEGABANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: MEGABANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49668/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  msrpc         Microsoft Windows RPC
49734/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: MONTEVERDE; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-07-22T14:03:34
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 140.49 seconds
```

### Crackmapexec 
haciendo una pequeña enumeración con crackmapexec por el servicio `smb` podemos identificar el sistema operativo de la maquina, el nombre y el dominio

```python
SO > Windows

Nombre > MONTEVERDE

DOMINIO > MEGABANK.LOCAL
```

![[Pasted image 20240722100520.png]]

`crackmapexec`
intentamos enumerar los recursos compartidos pero no tenemos permisos
![[Pasted image 20240722101054.png]]

### Enumeración del servicio smb
haciendo enumeraciones con 2 herramientas diferentes efectivamente no podemos enumerar el servicio smb
![[Pasted image 20240722101319.png]]

### rpcclient
haciendo la enumeración con `rpcclient` podemos hacer una enumeración de usuarios y de grupos. pero no podemos ver el usuario ni el grupo administrador
![[Pasted image 20240722101738.png]]

`Capturamos los usuarios`

```python
rpcclient -U "" 10.10.10.172 -N -c "enumdomusers" | grep -oP '\[.*?\]' | grep -v "0x" | tr -d '[]'
```

con el siguiente comando podemos enumerar los usuarios y limpiamos la `query` para que nos entregue el output limpio. obteniendo estos usuarios podemos intentar realizar un `ASREPRoast attack` 
![[Pasted image 20240722102730.png]]

### impacket-GetNPUsers
realizamos un ataque `ASREPRoast` pero no tenemos éxito. los usuarios no tienen configurado la opción `doesn't have UF_DONT_REQUIRE_PREAUTH set`
![[Pasted image 20240722103331.png]]

### crackmapexec
intentamos una autenticación por smb probando los usuarios que hemos conseguido. y usando los mismos usuarios como contraseñas obtenemos unas credenciales validas del usuario `SABatchJobs`
![[Pasted image 20240722103951.png]]

### smbmap
realizamos una validación con crackmapexec y nos aseguramos que podemos enumerar los recursos compartidos y con smbmap podemos listar sobre los recursos que tenemos permisos de lectura
![[Pasted image 20240722104616.png]]

`azure_uploads`
![[Pasted image 20240722105257.png]]

`Users$`
en este recurso compartido podemos encontrar otros directorios así que seguiremos enumerando
![[Pasted image 20240722105526.png]]

`azure.xml`
hemos encontrado un archivo `.xml` el cual puede ser muy interesante
![[Pasted image 20240722110858.png]]

`azure.xml`
en el archivo xml tenemos una contraseña que vamos a validar con el grupo de usuarios que ya tenemos
![[Pasted image 20240722121613.png]]

`crackmapexec`
validamos la nueva contraseña con el listado de usuarios que teníamos y obtenemos un nuevo usuario
![[Pasted image 20240722122128.png]]

### evil-winrm
logramos obtener una conexión con las credenciales que hemos encontrado 
![[Pasted image 20240722124558.png]]

### Escalada de privilegios
pertenecemos al grupo `Azure Admins` que es un grupo del dominio
![[Pasted image 20240722125101.png]]

`Microsoft Azure AD Sync`
esto es importante ya que podemos buscar vulnerabilidades que estén relacionadas a esto e intentar identificar si alguna es importante para nuestra explotación 
![[Pasted image 20240722125452.png]]

### AdDecrypt
investigando en internet encontramos un exploits el cual esta en el siguiente repositorio [AdSyncDecrypt](https://github.com/VbScrub/AdSyncDecrypt/releases) descargamos y descomprimimos el archivo
![[Pasted image 20240722130105.png]]

`upload`
en la maquina victima vamos a subir los archivos `AdDecrypt.exe` y `mcrypt.dll` 
![[Pasted image 20240722130512.png]]

`"C:\Program Files\Microsoft Azure AD Sync\Bin”` 
después de subir los archivos debemos dirigirnos a la ruta contemplada para poder realizar este ataque ya que de lo contrario no tendremos éxito
![[Pasted image 20240722130958.png]]





### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
