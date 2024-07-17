#ActiveDirectory #windows #tecnicas #hard 
### Ping

```python
ping -c 1 10.10.11.5
PING 10.10.11.5 (10.10.11.5) 56(84) bytes of data.
64 bytes from 10.10.11.5: icmp_seq=1 ttl=127 time=151 ms

--- 10.10.11.5 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 151.356/151.356/151.356/0.000 ms
```

### TTL = 127 Windows

### nmap

```python
# Nmap 7.94SVN scan initiated Tue Jul 16 20:13:18 2024 as: nmap -p- -open -sCV --min-rate 5000 -n -Pn -oN Scan 10.10.11.5
Nmap scan report for 10.10.11.5
Host is up (0.16s latency).
Not shown: 65511 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          nginx 1.25.5
|_http-server-header: nginx/1.25.5
|_http-title: Did not follow redirect to http://freelancer.htb/
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-07-17 05:13:41Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: freelancer.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc         Microsoft Windows RPC
49678/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
49790/tcp open  msrpc         Microsoft Windows RPC
55297/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-info: 
|   10.10.11.5\SQLEXPRESS: 
|     Instance name: SQLEXPRESS
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|     TCP port: 55297
|     Named pipe: \\10.10.11.5\pipe\MSSQL$SQLEXPRESS\sql\query
|_    Clustered: false
|_ssl-date: 2024-07-17T05:14:43+00:00; +5h00m00s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-07-17T05:11:10
|_Not valid after:  2054-07-17T05:11:10
| ms-sql-ntlm-info: 
|   10.10.11.5\SQLEXPRESS: 
|     Target_Name: FREELANCER
|     NetBIOS_Domain_Name: FREELANCER
|     NetBIOS_Computer_Name: DC
|     DNS_Domain_Name: freelancer.htb
|     DNS_Computer_Name: DC.freelancer.htb
|     DNS_Tree_Name: freelancer.htb
|_    Product_Version: 10.0.17763
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 4h59m59s, deviation: 0s, median: 4h59m59s
| smb2-time: 
|   date: 2024-07-17T05:14:32
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Jul 16 20:14:43 2024 -- 1 IP address (1 host up) scanned in 85.40 seconds
```

### Enumeración RPC - TCP 135 
por aquí no obtenemos nada así que seguiremos enumerando 
![[Pasted image 20240716195611.png]]

### Enumeración puerto 445 TCP
por aquí tampoco podemos conseguir nada ya que no tenemos credenciales 
![[Pasted image 20240716195853.png]]

### Enumeración del puerto 80 (http)
aquí podemos ver que tenemos dos registros diferentes uno como freelance y uno como employer
![[n7ffryqikmljjuemsffq.png]]

`employer register`
![[Pasted image 20240716203208.png]]

### QR-Code










### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
