#windows #medium #ActiveDirectory 
### Ping

```python
ping -c 1 10.10.10.169
PING 10.10.10.169 (10.10.10.169) 56(84) bytes of data.
64 bytes from 10.10.10.169: icmp_seq=1 ttl=127 time=156 ms

--- 10.10.10.169 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 156.178/156.178/156.178/0.000 ms
```

### TTL = 127 Windows

### nmap

```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-22 22:16 EDT
Nmap scan report for 10.10.10.169
Host is up (0.16s latency).
Not shown: 65511 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
53/tcp    open  domain       Simple DNS Plus
88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2024-07-23 02:23:49Z)
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp   open  ldap         Microsoft Windows Active Directory LDAP (Domain: megabank.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: MEGABANK)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: megabank.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf       .NET Message Framing
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49671/tcp open  msrpc        Microsoft Windows RPC
49678/tcp open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
49679/tcp open  msrpc        Microsoft Windows RPC
49684/tcp open  msrpc        Microsoft Windows RPC
49918/tcp open  msrpc        Microsoft Windows RPC
51262/tcp open  unknown
Service Info: Host: RESOLUTE; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: Resolute
|   NetBIOS computer name: RESOLUTE\x00
|   Domain name: megabank.local
|   Forest name: megabank.local
|   FQDN: Resolute.megabank.local
|_  System time: 2024-07-22T19:24:42-07:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
|_clock-skew: mean: 2h27m00s, deviation: 4h02m30s, median: 6m59s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-07-23T02:24:41
|_  start_date: 2024-07-23T01:45:53

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 89.43 seconds
```

### Enumeración del puerto 53 (Domain)
enumeración del dominio
![[Pasted image 20240722223223.png]]

### dnsrecon
el comando `dnsrecon` ha encontrado varios registros DNS y servicios asociados con el dominio `megabank.local` proporcionando información valiosa sobre la configuración y los servicios disponibles en ese dominio.
![[Pasted image 20240722223654.png]]

### Enumeración del puerto 135 (smrpc)
haciendo esta enumeración podemos 
![[Pasted image 20240722224736.png]]

`rpcclient -U "" 10.10.10.169 -N -c 'querydispinfo'`
con este comando descubrimos informacion relacionada con las credenciales del usuario `marko novak/Welcome123`
![[Pasted image 20240722225135.png]]

### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
