#ActiveDirectory #windows #medium 

### Machine Information

As is common in real life pentests, you will start the Puppy box with credentials for the following account: levi.james / KingofAkron2025!
### Ping
```python
ping -c 1 10.10.11.70
PING 10.10.11.70 (10.10.11.70) 56(84) bytes of data.
64 bytes from 10.10.11.70: icmp_seq=1 ttl=127 time=74.4 ms

--- 10.10.11.70 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 74.406/74.406/74.406/0.000 ms
```

### TTL 127 = Windows

### nmap
```python
sudo nmap -p- -open -sCV -sS -Pn -n 10.10.11.70
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-06 13:08 EDT
Stats: 0:04:10 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 68.18% done; ETC: 13:13 (0:00:26 remaining)
Nmap scan report for 10.10.11.70
Host is up (0.076s latency).
Not shown: 65513 filtered tcp ports (no-response)
Bug in iscsi-info: no string output.
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-06-07 00:11:57Z)
111/tcp   open  rpcbind       2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/tcp6  rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  2,3,4        111/udp6  rpcbind
|   100003  2,3         2049/udp   nfs
|   100003  2,3         2049/udp6  nfs
|   100005  1,2,3       2049/udp   mountd
|   100005  1,2,3       2049/udp6  mountd
|   100021  1,2,3,4     2049/tcp   nlockmgr
|   100021  1,2,3,4     2049/tcp6  nlockmgr
|   100021  1,2,3,4     2049/udp   nlockmgr
|   100021  1,2,3,4     2049/udp6  nlockmgr
|   100024  1           2049/tcp   status
|   100024  1           2049/tcp6  status
|   100024  1           2049/udp   status
|_  100024  1           2049/udp6  status
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
2049/tcp  open  nlockmgr      1-4 (RPC #100021)
3260/tcp  open  iscsi?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: PUPPY.HTB0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49681/tcp open  msrpc         Microsoft Windows RPC
51509/tcp open  msrpc         Microsoft Windows RPC
51549/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-06-07T00:13:53
|_  start_date: N/A
|_clock-skew: 6h59m58s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 367.88 seconds
```

### Información del dominio 

- Domain Name: PUPPY.HTB
- Host: DC

Además de los puertos y servicios comunes en un Active Directory, vemos 2 puertos para compartir archivos : SMB (puerto 445) : el servicio estándar para compartir archivos de Windows. NFS (puerto 2049) este servicio es poco común en los hosts de Windows, la presencia de NFS indica un subsistema UNIX o una configuración híbrida. por ejemplo, WSL2(windows subsystem for linux) o Docker para Windows con volúmenes compartidos. O una configuración errónea donde las configuraciones de origen Linux fueron portadas de manera insegura.

### nxc

Utilice el modulo de spider_plusmodo con netexec para enumerar todos los archivos legibles del SMB con las credenciales que se nos proporcionan inicialmente para el usuario del dominio levi.james
  
```python
nxc smb puppy.htb -u 'levi.james' -p 'KingofAkron2025!' -M spider_plus
```

![[Pasted image 20250606133200.png]]


- NETLOGON y SYSVOL, comúnmente utilizados en Active Directory para la entrega de scripts y la configuración de políticas, eran ambos legibles, pero en este caso no eran útiles.

- DEV es un recurso compartido personalizado. Sin embargo, no mostró permiso de lectura para el usuario "levi.james" en el contexto actual.

### smbclient

```python
smbclient //puppy.htb/DEV -U 'PUPPY.HTB\\levi.james'
```

![[Pasted image 20250606133725.png]]

### Bloodhound-python

```python
bloodhound-python -u 'levi.james' -p 'KingofAkron2025!' -d 'puppy.htb' -ns 10.10.11.70 --zip -c All -dc 'dc.puppy.htb'
```

![[Pasted image 20250606143647.png]]

Una vez generado el ZIP, tenemos que cargarlo en BloodHound. en este caso estoy usando el bloodhound-comunity para el análisis gráfico. Desde el principio el resultado expone una ruta de control de objeto saliente clara y definida: levi.jameses un miembro del HRgrupo, que tiene GenericWritederechos sobre el DEVELOPERSgrupo. Si bien BloodHound revela un puñado de otros vectores de ataque convincentes, desentrañaremos esos hilos a medida que se vuelvan tácticamente relevantes.

![[Pasted image 20250606144605.png]]