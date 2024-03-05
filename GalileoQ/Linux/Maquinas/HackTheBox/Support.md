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

![[Pasted image 20240304233251.png]]

