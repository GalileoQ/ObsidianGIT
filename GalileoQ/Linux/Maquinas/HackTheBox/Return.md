### Ping 
```python
ping -c 1 10.10.11.108
PING 10.10.11.108 (10.10.11.108) 56(84) bytes of data.
64 bytes from 10.10.11.108: icmp_seq=1 ttl=127 time=156 ms

--- 10.10.11.108 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 155.571/155.571/155.571/0.000 ms
```

### TTL 127= Windows

### nmap
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: HTB Printer Admin Panel
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-03-10 20:12:54Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
49674/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49675/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49717/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: PRINTER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: 17m41s
| smb2-time: 
|   date: 2024-03-10T20:13:51
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Mar 10 15:56:22 2024 -- 1 IP address (1 host up) scanned in 126.64 seconds
```

### Ports
| PORT      | STATE    | SERVICE      | VERSION                                               |
|-----------|----------|--------------|-------------------------------------------------------|
| 53/tcp    | open     | domain       | Simple DNS Plus                                       |
| 80/tcp    | open     | http         | Microsoft IIS httpd 10.0                              |
| 88/tcp    | open     | kerberos-sec | Microsoft Windows Kerberos (server time: 2024-03-10 20:12:54Z) |
| 135/tcp   | open     | msrpc        | Microsoft Windows RPC                                 |
| 139/tcp   | open     | netbios-ssn  | Microsoft Windows netbios-ssn                         |
| 389/tcp   | open     | ldap         | Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name) |
| 445/tcp   | open     | microsoft-ds | ?                                                     |
| 464/tcp   | open     | kpasswd5     | ?                                                     |
| 593/tcp   | open     | ncacn_http   | Microsoft Windows RPC over HTTP 1.0                   |
| 636/tcp   | open     | tcpwrapped   |                                                       |
| 3268/tcp  | open     | ldap         | Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name) |
| 3269/tcp  | open     | tcpwrapped   |                                                       |
| 5985/tcp  | open     | http         | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)              |
| 9389/tcp  | open     | mc-nmf       | .NET Message Framing                                  |
| 47001/tcp | open     | http         | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)              |
| 49664/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
| 49665/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
| 49666/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
| 49667/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
| 49671/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
| 49674/tcp | open     | ncacn_http   | Microsoft Windows RPC over HTTP 1.0                   |
| 49675/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
| 49676/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
| 49679/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
| 49717/tcp | open     | msrpc        | Microsoft Windows RPC                                 |
Service Info: Host: PRINTER; OS: Windows; CPE: cpe:/o:microsoft:windows

### Enumeracion del puerto SMB (445)
tenemos recursos compartidos pero no tenemos permisos para leerlos
![[Pasted image 20240310172159.png]]

### Enumeración del puerto http (80)

![[Pasted image 20240310172404.png]]


![[Pasted image 20240310172444.png]]