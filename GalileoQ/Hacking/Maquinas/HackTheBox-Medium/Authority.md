#ActiveDirectory #medium #windows 

### Ping
```python
PING 10.10.11.222 (10.10.11.222) 56(84) bytes of data.
64 bytes from 10.10.11.222: icmp_seq=1 ttl=127 time=153 ms

--- 10.10.11.222 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 153.116/153.116/153.116/0.000 ms
```

### TTL 127=Windows

### nmap
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-03-23 04:30:54Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
|_ssl-date: 2024-03-23T04:32:01+00:00; +3h59m03s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Not valid before: 2022-08-09T23:03:21
|_Not valid after:  2024-08-09T23:13:21
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Not valid before: 2022-08-09T23:03:21
|_Not valid after:  2024-08-09T23:13:21
|_ssl-date: 2024-03-23T04:32:04+00:00; +3h59m02s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Not valid before: 2022-08-09T23:03:21
|_Not valid after:  2024-08-09T23:13:21
|_ssl-date: 2024-03-23T04:32:01+00:00; +3h59m02s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Not valid before: 2022-08-09T23:03:21
|_Not valid after:  2024-08-09T23:13:21
|_ssl-date: 2024-03-23T04:32:04+00:00; +3h59m02s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
8443/tcp  open  ssl/https-alt
|_ssl-date: TLS randomness does not represent time
|_http-title: Site doesn't have a title (text/html;charset=ISO-8859-1).
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 200 
|     Content-Type: text/html;charset=ISO-8859-1
|     Content-Length: 82
|     Date: Sat, 23 Mar 2024 04:31:02 GMT
|     Connection: close
|     <html><head><meta http-equiv="refresh" content="0;URL='/pwm'"/></head></html>
|   GetRequest: 
|     HTTP/1.1 200 
|     Content-Type: text/html;charset=ISO-8859-1
|     Content-Length: 82
|     Date: Sat, 23 Mar 2024 04:31:01 GMT
|     Connection: close
|     <html><head><meta http-equiv="refresh" content="0;URL='/pwm'"/></head></html>
|   HTTPOptions: 
|     HTTP/1.1 200 
|     Allow: GET, HEAD, POST, OPTIONS
|     Content-Length: 0
|     Date: Sat, 23 Mar 2024 04:31:01 GMT
|     Connection: close
|   RTSPRequest: 
|     HTTP/1.1 400 
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 1936
|     Date: Sat, 23 Mar 2024 04:31:08 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 400 
|     Request</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 400 
|_    Request</h1><hr class="line" /><p><b>Type</b> Exception Report</p><p><b>Message</b> Invalid character found in the HTTP protocol [RTSP&#47;1.00x0d0x0a0x0d0x0a...]</p><p><b>Description</b> The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid
| ssl-cert: Subject: commonName=172.16.2.118
| Not valid before: 2024-03-21T00:17:23
|_Not valid after:  2026-03-23T11:55:47
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  msrpc         Microsoft Windows RPC
49682/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49683/tcp open  msrpc         Microsoft Windows RPC
49684/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  msrpc         Microsoft Windows RPC
49692/tcp open  msrpc         Microsoft Windows RPC
49695/tcp open  msrpc         Microsoft Windows RPC
49758/tcp open  msrpc         Microsoft Windows RPC
52388/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8443-TCP:V=7.94SVN%T=SSL%I=7%D=3/22%Time=65FE22FE%P=x86_64-pc-linux
SF:-gnu%r(GetRequest,DB,"HTTP/1\.1\x20200\x20\r\nContent-Type:\x20text/htm
SF:l;charset=ISO-8859-1\r\nContent-Length:\x2082\r\nDate:\x20Sat,\x2023\x2
SF:0Mar\x202024\x2004:31:01\x20GMT\r\nConnection:\x20close\r\n\r\n\n\n\n\n
SF:\n<html><head><meta\x20http-equiv=\"refresh\"\x20content=\"0;URL='/pwm'
SF:\"/></head></html>")%r(HTTPOptions,7D,"HTTP/1\.1\x20200\x20\r\nAllow:\x
SF:20GET,\x20HEAD,\x20POST,\x20OPTIONS\r\nContent-Length:\x200\r\nDate:\x2
SF:0Sat,\x2023\x20Mar\x202024\x2004:31:01\x20GMT\r\nConnection:\x20close\r
SF:\n\r\n")%r(FourOhFourRequest,DB,"HTTP/1\.1\x20200\x20\r\nContent-Type:\
SF:x20text/html;charset=ISO-8859-1\r\nContent-Length:\x2082\r\nDate:\x20Sa
SF:t,\x2023\x20Mar\x202024\x2004:31:02\x20GMT\r\nConnection:\x20close\r\n\
SF:r\n\n\n\n\n\n<html><head><meta\x20http-equiv=\"refresh\"\x20content=\"0
SF:;URL='/pwm'\"/></head></html>")%r(RTSPRequest,82C,"HTTP/1\.1\x20400\x20
SF:\r\nContent-Type:\x20text/html;charset=utf-8\r\nContent-Language:\x20en
SF:\r\nContent-Length:\x201936\r\nDate:\x20Sat,\x2023\x20Mar\x202024\x2004
SF::31:08\x20GMT\r\nConnection:\x20close\r\n\r\n<!doctype\x20html><html\x2
SF:0lang=\"en\"><head><title>HTTP\x20Status\x20400\x20\xe2\x80\x93\x20Bad\
SF:x20Request</title><style\x20type=\"text/css\">body\x20{font-family:Taho
SF:ma,Arial,sans-serif;}\x20h1,\x20h2,\x20h3,\x20b\x20{color:white;backgro
SF:und-color:#525D76;}\x20h1\x20{font-size:22px;}\x20h2\x20{font-size:16px
SF:;}\x20h3\x20{font-size:14px;}\x20p\x20{font-size:12px;}\x20a\x20{color:
SF:black;}\x20\.line\x20{height:1px;background-color:#525D76;border:none;}
SF:</style></head><body><h1>HTTP\x20Status\x20400\x20\xe2\x80\x93\x20Bad\x
SF:20Request</h1><hr\x20class=\"line\"\x20/><p><b>Type</b>\x20Exception\x2
SF:0Report</p><p><b>Message</b>\x20Invalid\x20character\x20found\x20in\x20
SF:the\x20HTTP\x20protocol\x20\[RTSP&#47;1\.00x0d0x0a0x0d0x0a\.\.\.\]</p><
SF:p><b>Description</b>\x20The\x20server\x20cannot\x20or\x20will\x20not\x2
SF:0process\x20the\x20request\x20due\x20to\x20something\x20that\x20is\x20p
SF:erceived\x20to\x20be\x20a\x20client\x20error\x20\(e\.g\.,\x20malformed\
SF:x20request\x20syntax,\x20invalid\x20");
Service Info: Host: AUTHORITY; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 3h59m02s, deviation: 0s, median: 3h59m01s
| smb2-time: 
|   date: 2024-03-23T04:31:54
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Mar 22 20:33:02 2024 -- 1 IP address (1 host up) scanned in 97.79 seconds
```
###### Active Directory LDAP (Domain: authority.htb) - (DNS authority.htb.corp)

### Enumeración del puerto 53 (Domain)
`dig` no obtenemos nada relativamente importante
![[Pasted image 20240322204510.png]]

`dnsrecon`
![[Pasted image 20240322205956.png]]

### Enumeración del puerto 80 (http)
en la web no obtenemos mucha información
![[Pasted image 20240322210109.png]]

### Enumeración del puerto 88 (Kerberos)
por el momento no podemos obtener ningún usuario para el servicio de kerberos
![[Pasted image 20240322212639.png]]

### Enumeración del puerto 135 (msrpc)
por el momento seguimos sin obtener información importante
![[Pasted image 20240322213419.png]]

### Enumeración del puerto 389 (ldap)
seguimos sin obtener informacion
```python
ldapsearch -x -H ldap://10.10.11.222 -b "dc=authority,dc=thb" -s sub "(objectclass=*)"
```
![[Pasted image 20240322215026.png]]

### Enumeración del puerto 445 (SMB)
obtenemos información sobre dos recursos compartidos en los cuales tenemos permisos de lectura en este caso `Development` parece interesante
![[Pasted image 20240322215618.png]]

### smbclient
enumeramos diferentes recursos compartidos asi que vamos a mirarlos
![[Pasted image 20240322222019.png]]

con el comando `-c "recurse; prompt; mget * "` podemos descargar el recurso compartido completo
![[Pasted image 20240322223213.png]]

### Code
al analizar los recursos que hemos descargado conseguimos estos VAULT que parecen interesantes
![[Pasted image 20240322230509.png]]

