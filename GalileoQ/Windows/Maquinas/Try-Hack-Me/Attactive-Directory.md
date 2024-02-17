### nmap
```python
sudo nmap -p- --open -sS -sC -sV --min-rate 5000 -n -Pn 10.10.124.3
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-02-17 17:22 EST
Nmap scan report for 10.10.124.3
Host is up (0.19s latency).
Not shown: 65452 closed tcp ports (reset), 57 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-02-17 22:21:59Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2024-02-17T22:23:06+00:00; -40s from scanner time.
| ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
| Not valid before: 2024-02-16T22:18:45
|_Not valid after:  2024-08-17T22:18:45
| rdp-ntlm-info: 
|   Target_Name: THM-AD
|   NetBIOS_Domain_Name: THM-AD
|   NetBIOS_Computer_Name: ATTACKTIVEDIREC
|   DNS_Domain_Name: spookysec.local
|   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-02-17T22:22:58+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49676/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49684/tcp open  msrpc         Microsoft Windows RPC
49695/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -40s, deviation: 0s, median: -40s
| smb2-time: 
|   date: 2024-02-17T22:22:57
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.22 seconds
```

### Ports: 53-80-88-135-139-389-445-464-593-636-3268-3269-3389-5985-9389-47001-49664-49665-49669-49673-9675-49676-49679-49684-49685
###### El puerto 88 nos indica que esta corriendo el protocolo kerberos
###### el puerto 389 nos indica que tenemos una redireccion hacia un domino llamado "spookysec.local"

### Enumeracion del puerto 80
no tenemos mucha informacion del puerto 80
![[Pasted image 20240217183752.png]]
###### vamos a agregar el dominio "spookysec.local"al /etc/hosts

### crackmapexec
esta herramienta nos permite enumerar el puerto smb para obtener un poco de información 
![[Pasted image 20240217184401.png]]

### enum4linux 
otra herramienta para enumerar. en este caso no encontramos nada.
![[Pasted image 20240217185818.png]]

### Kerbrute
vamos a utilizar esta herramienta llamada kerbrute. haremos un git clone del repositorio de github 
![[Pasted image 20240217190205.png]]

###### una ves clonado el repositorio vamos al directorio para instalar los requierements
![[Pasted image 20240217190539.png]]
###### esta herramienta necesita un diccionario de usuarios y uno de contraseñas para poder funcionar
```python
	python3 kerbrute.py -users userlist.txt -passwords passwordlist.txt -domain spookysec.local -t 100
	
	./kerbrute_linux_amd64 userenum --dc 10.10.124.3 -d spookysec.local userlist.txt
```


