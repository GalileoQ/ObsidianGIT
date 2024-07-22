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

![[Pasted image 20240722102730.png]]


### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
