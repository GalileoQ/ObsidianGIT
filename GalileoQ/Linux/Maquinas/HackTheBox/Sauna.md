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

![[Pasted image 20240313005637.png]]