#windows  #tecnicas #Insane 
### Ping

```python
ping -c 1 10.10.11.24
PING 10.10.11.24 (10.10.11.24) 56(84) bytes of data.
64 bytes from 10.10.11.24: icmp_seq=1 ttl=127 time=156 ms

--- 10.10.11.24 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 156.384/156.384/156.384/0.000 ms
```

### TTL = 127 Windows

### nmap

```python
# Nmap 7.94SVN scan initiated Thu Jul 18 17:18:48 2024 as: nmap -p- -open -sCV --min-rate 5000 -n -Pn -oN Scan 10.10.11.24
Nmap scan report for 10.10.11.24
Host is up (0.16s latency).
Not shown: 65508 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-07-18 21:19:21Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: ghost.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Subject Alternative Name: DNS:DC01.ghost.htb, DNS:ghost.htb
| Not valid before: 2024-06-19T15:45:56
|_Not valid after:  2124-06-19T15:55:55
443/tcp   open  https?
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: ghost.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Subject Alternative Name: DNS:DC01.ghost.htb, DNS:ghost.htb
| Not valid before: 2024-06-19T15:45:56
|_Not valid after:  2124-06-19T15:55:55
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2022 16.00.1000.00; RC0+
| ms-sql-ntlm-info: 
|   10.10.11.24:1433: 
|     Target_Name: GHOST
|     NetBIOS_Domain_Name: GHOST
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: ghost.htb
|     DNS_Computer_Name: DC01.ghost.htb
|     DNS_Tree_Name: ghost.htb
|_    Product_Version: 10.0.20348
|_ssl-date: 2024-07-18T21:21:01+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-07-18T13:28:51
|_Not valid after:  2054-07-18T13:28:51
| ms-sql-info: 
|   10.10.11.24:1433: 
|     Version: 
|       name: Microsoft SQL Server 2022 RC0+
|       number: 16.00.1000.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RC0
|       Post-SP patches applied: true
|_    TCP port: 1433
2179/tcp  open  vmrdp?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: ghost.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Subject Alternative Name: DNS:DC01.ghost.htb, DNS:ghost.htb
| Not valid before: 2024-06-19T15:45:56
|_Not valid after:  2124-06-19T15:55:55
|_ssl-date: TLS randomness does not represent time
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: ghost.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Subject Alternative Name: DNS:DC01.ghost.htb, DNS:ghost.htb
| Not valid before: 2024-06-19T15:45:56
|_Not valid after:  2124-06-19T15:55:55
|_ssl-date: TLS randomness does not represent time
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Not valid before: 2024-06-16T15:49:55
|_Not valid after:  2024-12-16T15:49:55
|_ssl-date: 2024-07-18T21:21:01+00:00; -1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: GHOST
|   NetBIOS_Domain_Name: GHOST
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: ghost.htb
|   DNS_Computer_Name: DC01.ghost.htb
|   DNS_Tree_Name: ghost.htb
|   Product_Version: 10.0.20348
|_  System_Time: 2024-07-18T21:20:23+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
8008/tcp  open  http          nginx 1.18.0 (Ubuntu)
|_http-title: Ghost
| http-robots.txt: 5 disallowed entries 
|_/ghost/ /p/ /email/ /r/ /webmentions/receive/
|_http-generator: Ghost 5.78
|_http-server-header: nginx/1.18.0 (Ubuntu)
8443/tcp  open  ssl/http      nginx 1.18.0 (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-title: Ghost Core
|_Requested resource was /login
| ssl-cert: Subject: commonName=core.ghost.htb
| Subject Alternative Name: DNS:core.ghost.htb
| Not valid before: 2024-06-18T15:14:02
|_Not valid after:  2124-05-25T15:14:02
| tls-nextprotoneg: 
|_  http/1.1
9389/tcp  open  mc-nmf        .NET Message Framing
49443/tcp open  unknown
49664/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
50221/tcp open  msrpc         Microsoft Windows RPC
60539/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
60752/tcp open  msrpc         Microsoft Windows RPC
60785/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OSs: Windows, Linux; CPE: cpe:/o:microsoft:windows, cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2024-07-18T21:20:18
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jul 18 17:21:05 2024 -- 1 IP address (1 host up) scanned in 137.62 seconds
```

### Enumeración del puerto 8008 (http)
Ghost CMS es un sistema de gestión de contenidos (CMS) moderno y de código abierto diseñado principalmente para blogs (el resultado de Nmap indica que este es el 5.78 versión)

![[Pasted image 20240718174434.png]]

```python

1)Panel de administrador:  
  
/ghost/ - La página de inicio de sesión de administrador.  
/ghost/#/signin - Enlace directo a la página de inicio de sesión.  
  
2)Contenido y archivos estáticos:  
  
/content/images/ - Directorio predeterminado para imágenes cargadas.  
/content/themes/ - Directorio de temas instalados.  
/content/ - Directorio base de contenidos, temas e imágenes.  
  
3)Puntos finales de API:  
  
/ghost/api/v3/ - Punto final API para Ghost CMS v3.  
/ghost/api/v4/ - Punto final API para Ghost CMS v4.  
/ghost/api/v4/content/ - Punto final de API de contenido público.  
/ghost/api/v4/admin/ - Punto final de API de administración.  
  
4)Archivos de configuración y respaldo:  
  
/config.production.json - Archivo de configuración para el entorno de producción (no debe ser de acceso público).  
/config.development.json - Archivo de configuración del entorno de desarrollo.  
/ghost/api/v3/admin/db/ - Punto final de base de datos potencialmente accesible para operaciones de copia de seguridad y restauración.
```

`<script src="/assets/built/source.js?v=f335afc3d4"></script>`
Al inspeccionar su código fuente, podemos descubrir un punto final de JavaScript:
![[Pasted image 20240718174936.png]]

### Fuzzing con wfuzz
haciendo un Fuzzing de directorios no podemos encontrar nada muy importante
![[Pasted image 20240718175430.png]]

### Fuzzing con ffuf
encontramos un subdominio así que vamos a enumerarlo
![[Pasted image 20240718191759.png]]

### intranet
hemos conseguido un panel de login. aquí empieza lo bueno 
![[Pasted image 20240718191926.png]]

### Ghost Core
investigando en internet algunos foros de la comunidad del `CMS Ghost` nos hacen referencia a un subdominio llamado `core`
![[Pasted image 20240718192311.png]]

`federeacion`
al hacer clic en el botón nos redirige a otro subdominio así que lo vamos a agregar 
![[Pasted image 20240718192530.png]]

`login`
volvemos al inicio e intentamos iniciar sesión para interceptar con Burpsuite 
![[Pasted image 20240718193135.png]]

### Burpsuite
después de varios intentos identificamos que el identificador `*` tanto en el username como en el secret nos permite obtener un token de sesión 
![[Pasted image 20240718193318.png]]

### Intranet
iniciamos sesión como el usuario `Kathryn.holland`
![[Pasted image 20240718193515.png]]

`Users`
podemos enumerar el listado de usuarios con los roles
![[Pasted image 20240718193754.png]]

### Kerbrute
usando la lista de usuarios hemos intentado hacer una enumeración con kerbrute pero no obtenemos nada. sin embargo investigando p
![[Pasted image 20240718194729.png]]

