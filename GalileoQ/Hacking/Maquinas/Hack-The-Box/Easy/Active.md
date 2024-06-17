#windows #ActiveDirectory #Easy 
### Ping
```python
ping -c 1 10.10.10.100
PING 10.10.10.100 (10.10.10.100) 56(84) bytes of data.
64 bytes from 10.10.10.100: icmp_seq=1 ttl=127 time=156 ms

--- 10.10.10.100 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 155.595/155.595/155.595/0.000 ms
```

### nmap
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-03-19 02:02:07Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5722/tcp  open  msrpc         Microsoft Windows RPC
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         Microsoft Windows RPC
49165/tcp open  msrpc         Microsoft Windows RPC
49170/tcp open  msrpc         Microsoft Windows RPC
49171/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-03-19T02:03:06
|_  start_date: 2024-03-18T13:40:44
|_clock-skew: -55s
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 93.29 seconds
```

### crackmapexec
enumeramos el sistema para ver a que nos estamos enfrentando. en este caso es un DC (Domain Controller) con el nombre active.htb
![[Pasted image 20240611200347.png]]
### smbmap
`smbclient` para enumerar los archivos compartidos. esto lo podemos confirmar con `smbmap` en este caso nos reporta los permisos que tenemos sobre los archivos compartidos  
![[Pasted image 20240611200357.png]]

`Enumeracion`
![[Pasted image 20240611200406.png]]

`enumerando el systema hemos conceguido un archivo.xml`
![[Pasted image 20240611200419.png]]

### gpp-decrypte
en el archivo xml podemos ver una contraseña encriptada y un usuario. con la herramienta `gpp-decrypt` podemos obtener la contraseña en texto claro
![[Pasted image 20240611200430.png]]

### crackmapexec
validamos las credenciales que hemos conseguido a nivel de `SMB` y tambien para `winrm` para este ultimo no tenemos acceso 
![[Pasted image 20240611200438.png]]

### smbmap
vamos a listar de nuevo los recursos compartidos con las credenciales obtenidas
![[Pasted image 20240611200445.png]]

`Enumeracion de los recursos compartidos`
![[Pasted image 20240611200455.png]]

### rpcclient 
enumeración de usuarios, grupos y la información con `rpcclient`
![[Pasted image 20240611200504.png]]

### impacket-GetNPUsers
intentamos obtener un `TGT` pero el usuario no tiene `UF_DONT_REQUIRE_PREAUTH` por lo cual no podremos obtener un `TGT`
![[Pasted image 20240611200514.png]]

### impacket-GetUserSPNs 
intentamos obtener un `TGS (Ticket-granting server)` TGS emite tickets de servicio que permiten a los usuarios acceder a servicios específicos en la red una vez que han sido autenticados correctamente.
![[Pasted image 20240611200522.png]]

### john
guardamos el hash en un archivo y lo intentamos crackear con `john`
![[Pasted image 20240611200536.png]]

### crackmapexec 
validamos las credenciales que hemos conseguido tanto para `SMB` como para `winrm`. en este caso no tenemos conexión por `evil-winrm`
![[Pasted image 20240611200544.png]]

### impacket-psexec
establecemos conexión con `impacket-psexec`
![[Pasted image 20240611200550.png]]

### WE ARE ROOT