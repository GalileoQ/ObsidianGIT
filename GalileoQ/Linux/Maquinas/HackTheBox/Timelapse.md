### Ping 
```python
ping -c 1 10.10.11.152
PING 10.10.11.152 (10.10.11.152) 56(84) bytes of data.
64 bytes from 10.10.11.152: icmp_seq=1 ttl=127 time=150 ms

--- 10.10.11.152 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 149.821/149.821/149.821/0.000 ms
```

### nmap
```python
PORT      STATE SERVICE           VERSION
53/tcp    open  domain            Simple DNS Plus
88/tcp    open  kerberos-sec      Microsoft Windows Kerberos (server time: 2024-03-09 09:20:22Z)
135/tcp   open  msrpc             Microsoft Windows RPC
139/tcp   open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp   open  ldap              Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ldapssl?
3268/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
3269/tcp  open  globalcatLDAPssl?
5986/tcp  open  ssl/http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
| ssl-cert: Subject: commonName=dc01.timelapse.htb
| Not valid before: 2021-10-25T14:05:29
|_Not valid after:  2022-10-25T14:25:29
| tls-alpn: 
|_  http/1.1
|_http-server-header: Microsoft-HTTPAPI/2.0
|_ssl-date: 2024-03-09T09:21:54+00:00; +7h59m09s from scanner time.
9389/tcp  open  mc-nmf            .NET Message Framing
49667/tcp open  msrpc             Microsoft Windows RPC
49673/tcp open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc             Microsoft Windows RPC
49718/tcp open  msrpc             Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: 7h59m08s, deviation: 0s, median: 7h59m08s
| smb2-time: 
|   date: 2024-03-09T09:21:13
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 239.62 seconds
```
con este reconocimiento logramos obtener el nombre de la maquina: Service Info: Host: DC01 tambien tenemos el nombre del dominio: Domain: timelapse.htb y tambien tenemos el puerto SMB abierto
### Ports
| PORT      | STATE | SERVICE           | VERSION                                                                                          |
| --------- | ----- | ----------------- | ------------------------------------------------------------------------------------------------ |
| 53/tcp    | open  | domain            | Simple DNS Plus                                                                                  |
| 88/tcp    | open  | kerberos-sec      | Microsoft Windows Kerberos (server time: 2024-03-09 09:20:22Z)                                   |
| 135/tcp   | open  | msrpc             | Microsoft Windows RPC                                                                            |
| 139/tcp   | open  | netbios-ssn       | Microsoft Windows netbios-ssn                                                                    |
| 389/tcp   | open  | ldap              | Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name) |
| 445/tcp   | open  | microsoft-ds?     |                                                                                                  |
| 464/tcp   | open  | kpasswd5?         |                                                                                                  |
| 593/tcp   | open  | ncacn_http        | Microsoft Windows RPC over HTTP 1.0                                                              |
| 636/tcp   | open  | ldapssl?          |                                                                                                  |
| 3268/tcp  | open  | ldap              | Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name) |
| 3269/tcp  | open  | globalcatLDAPssl? |                                                                                                  |
| 5986/tcp  | open  | ssl/http          | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)                                                          |
| 9389/tcp  | open  | mc-nmf            | .NET Message Framing                                                                             |
| 49667/tcp | open  | msrpc             | Microsoft Windows RPC                                                                            |
| 49673/tcp | open  | ncacn_http        | Microsoft Windows RPC over HTTP 1.0                                                              |
| 49674/tcp | open  | msrpc             | Microsoft Windows RPC                                                                            |
| 49718/tcp | open  | msrpc             | Microsoft Windows RPC                                                                            |

### Enumeración del puerto SMB

```python
smbclient -L //10.10.10.10/
```

![[Pasted image 20240308220917.png]]

### descargamos todos los archivos que estan en los directorios compartidos 

![[Pasted image 20240308222108.png]]

### zip2john

![[Pasted image 20240308221822.png]]
obtenemos credenciales para descomprimir el archivo

### usamos openssl para extraer el nuevo archivo 
parece que necesitamos una password diferente para este archivo
![[Pasted image 20240308223006.png]]

convertimos el archivo pfx a un formato hash y luego usamos la herramienta pfx2john, John para descifrar la contraseña.

![[Pasted image 20240308224352.png]]

### openssl
usaremos openssl para generar una clave y un certificado validos. usaremos la clave que hemos conseguido anteriormente 
![[Pasted image 20240308232530.png]]

esto nos crea un certificado y una clave validos
![[Pasted image 20240308234011.png]]

### Evil-WinRM
al desciframos el archivo pfx generamos una clave y un certificado válidos, podemos intentar iniciar sesión a través de Evil-WinRM esto nos permite pasar una clave y un certificado usando los indicadores c y k 
![[Pasted image 20240309000213.png]]

### ConsoleHost_history.txt
este archivo basicamente se utiliza para mostrar el contenido del historial de comandos de PowerShell
![[Pasted image 20240309000757.png]]
tenemos credenciales asi que usaremos esto para crear una nueva conexión

![[Pasted image 20240309001757.png]]

### Escalada de privilegios

![[Pasted image 20240309001935.png]]
El resultado del comando anterior muestra que somos parte del grupo LAPS_Readers . (LAPS) se utiliza para administrar las contraseñas de cuentas locales de las computadoras con Active Directory. Hay un módulo de PowerShell que se puede usar para recuperar esta contraseña se llama Get-LAPSPasswords [[https://github.com/kfosaaen/Get-LAPSPasswords]]

subimos el archivo 
![[Pasted image 20240309004319.png]]

### GET-LAPS
tenemos credenciales que podriamos probar para administrador
![[Pasted image 20240309013840.png]]

### evil-winrm

![[Pasted image 20240309015511.png]]