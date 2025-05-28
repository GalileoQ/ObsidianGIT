#ActiveDirectory #windows #Easy 
# MACHINE INFORMATION

As is common in real life Windows pentests, you will start the Fluffy box with credentials for the following account: j.fleischman / J0elTHEM4n1990!
### ping

```python
❯ ping -c 1 10.10.11.69
PING 10.10.11.69 (10.10.11.69) 56(84) bytes of data.
64 bytes from 10.10.11.69: icmp_seq=1 ttl=127 time=129 ms

--- 10.10.11.69 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 128.552/128.552/128.552/0.000 ms
```

### nmap

```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-05-28 22:24:39Z)
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.fluffy.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.fluffy.htb
| Not valid before: 2025-04-17T16:04:17
|_Not valid after:  2026-04-17T16:04:17
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.fluffy.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.fluffy.htb
| Not valid before: 2025-04-17T16:04:17
|_Not valid after:  2026-04-17T16:04:17
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49686/tcp open  msrpc         Microsoft Windows RPC
49695/tcp open  msrpc         Microsoft Windows RPC
49700/tcp open  msrpc         Microsoft Windows RPC
49711/tcp open  msrpc         Microsoft Windows RPC
49723/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-05-28T22:25:26
|_  start_date: N/A
|_clock-skew: 6h59m58s
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed May 28 10:26:37 2025 -- 1 IP address (1 host up) scanned in 193.77 seconds
```

### Enumeración SMB 

### Rid-Brute

```python
❯ crackmapexec smb fluffy.htb -u j.fleischman -p 'J0elTHEM4n1990!' --rid-brute
```

![[Pasted image 20250528120635.png]]

Usando la --shares, enumeramos los derechos de acceso a los recursos compartidos SMB disponibles

```python
❯ nxc smb dc01.fluffy.htb -u 'j.fleischman' -p 'J0elTHEM4n1990!' --shares
```

![[Pasted image 20250528121206.png]]

Confirmamos el acceso de lectura y escritura a un recurso compartido personalizado llamado IT y descargamos todo con mget * .

![[Pasted image 20250528122155.png]]