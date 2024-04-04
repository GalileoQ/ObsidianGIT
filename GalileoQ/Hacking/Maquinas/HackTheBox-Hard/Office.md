#ActiveDirectory #windows #fuzzing 
### Ping
```python
ping -c 1 10.10.11.3
PING 10.10.11.3 (10.10.11.3) 56(84) bytes of data.
64 bytes from 10.10.11.3: icmp_seq=1 ttl=127 time=153 ms

--- 10.10.11.3 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 153.009/153.009/153.009/0.000 ms
```

### TTL 127 = Windows

### nmap
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Apache httpd 2.4.56 ((Win64) OpenSSL/1.1.1t PHP/8.0.28)
| http-robots.txt: 16 disallowed entries (15 shown)
| /joomla/administrator/ /administrator/ /api/ /bin/ 
| /cache/ /cli/ /components/ /includes/ /installation/ 
|_/language/ /layouts/ /libraries/ /logs/ /modules/ /plugins/
|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28
|_http-generator: Joomla! - Open Source Content Management
|_http-title: Home
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-04-04 10:01:53Z)
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: office.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC.office.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC.office.htb
| Not valid before: 2023-05-10T12:36:58
|_Not valid after:  2024-05-09T12:36:58
443/tcp   open  ssl/http      Apache httpd 2.4.56 (OpenSSL/1.1.1t PHP/8.0.28)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_http-title: 403 Forbidden
| tls-alpn: 
|_  http/1.1
|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: office.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC.office.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC.office.htb
| Not valid before: 2023-05-10T12:36:58
|_Not valid after:  2024-05-09T12:36:58
|_ssl-date: TLS randomness does not represent time
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: office.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC.office.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC.office.htb
| Not valid before: 2023-05-10T12:36:58
|_Not valid after:  2024-05-09T12:36:58
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: office.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC.office.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC.office.htb
| Not valid before: 2023-05-10T12:36:58
|_Not valid after:  2024-05-09T12:36:58
|_ssl-date: TLS randomness does not represent time
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
51349/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
51352/tcp open  msrpc         Microsoft Windows RPC
61261/tcp open  msrpc         Microsoft Windows RPC
61265/tcp open  msrpc         Microsoft Windows RPC
Service Info: Hosts: DC, www.example.com; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-04-04T10:02:51
|_  start_date: N/A
|_clock-skew: 7h58m55s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 142.64 seconds
```

### Dominio
tenemos el nombre del dominio y el DNS así que lo agregaremos al host
![[Pasted image 20240403221531.png]]

### Enumeración del puerto 53 (Domain)
podemos ver una cookie que podríamos guardar
![[Pasted image 20240403222134.png]]

`dnsenum`
probamos con dnsenum pero tampoco podemos enumerar nada
![[Pasted image 20240403223945.png]]

### Enumeración del puerto 80 (http)

![[Pasted image 20240403222016.png]]

### Fuzzing con ffuz
obtenemos un montón de directorios. administrator parece interesante
![[Pasted image 20240403231931.png]]

`administrator`
obtenemos el panel de login pero no tenemos credenciales validas 
![[Pasted image 20240403232007.png]]

`manifests`
![[Pasted image 20240403234159.png]]

`manifests`
encontramos un subdirectorio dentro de administrator llamado manifests. vamos a enumerar
![[Pasted image 20240403234220.png]]

`http://office.htb/administrator/manifests/files/joomla.xml`
enumerando los archivos llegamos a esta ruta donde conseguimos un archivo joomla.xml que nos muestra la versión del CMS
![[Pasted image 20240403234348.png]]

### Vulnerabilidad CVE-2023-23752 joomla
Es un bypass de autenticación que resulta en una fuga de información en los servidores Joomla!.
![[Pasted image 20240403235334.png]]

`descarga del repositorio`
```python
$ git clone https://github.com/Pari-Malam/CVE-2023-23752
$ cd CVE-2023-23752
$ pip/pip3 install -r requirements.txt
$ python/python3 joomla.py
```

`pip install`
![[Pasted image 20240403235224.png]]

### joomla.py
este exploit aprovecha una vulnerabilidad en joomlab lo cual permite leer la base de datos y obtener las credenciales del usuario root
![[Pasted image 20240403235809.png]]

`credenciales`
las credenciales que hemos conseguido no son validas para ingresar al CMS
![[Pasted image 20240404000232.png]]

### kerbrute
vamos a enumerar usuarios utilizando kerbrute
![[Pasted image 20240404110458.png]]

