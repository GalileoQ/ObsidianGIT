# 
### Ping

```python

```

### TTL = 

### nmap

```python
> sudo nmap -p- -open -sCV --script smb-vuln-ms17-010 --min-rate 5000 -n -Pn 10.10.11.21 -oN Scan

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-27 19:03 EDT
Nmap scan report for 10.10.11.21
Host is up (0.15s latency).
Not shown: 65512 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
25/tcp    open  smtp          hMailServer smtpd
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-06-27 23:04:27Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: axlle.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: axlle.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
9389/tcp  open  mc-nmf        .NET Message Framing
49169/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49170/tcp open  unknown
49175/tcp open  unknown
49664/tcp open  unknown
49668/tcp open  unknown
52440/tcp open  unknown
61423/tcp open  unknown
Service Info: Host: MAINFRAME; OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 123.68 seconds
```

### Enumeración 


### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
