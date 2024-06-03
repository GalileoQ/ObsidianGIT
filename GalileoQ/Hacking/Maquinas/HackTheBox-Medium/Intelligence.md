#ActiveDirectory #medium #tecnicas #windows #role 
### Ping
```python
ping -c 1 10.10.10.248
PING 10.10.10.248 (10.10.10.248) 56(84) bytes of data.
64 bytes from 10.10.10.248: icmp_seq=1 ttl=127 time=149 ms

--- 10.10.10.248 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 149.231/149.231/149.231/0.000 ms
```

### TTL 127 = Windows

### nmap
```python
# Nmap 7.94SVN scan initiated Mon Jun  3 11:00:07 2024 as: nmap -p- --open -sC -sV --min-rate 3000 -n -Pn -oN Scan 10.10.10.248
Nmap scan report for 10.10.10.248
Host is up (0.15s latency).
Not shown: 65515 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Intelligence
| http-methods: 
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-06-03 22:01:00Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-06-03T22:02:33+00:00; +6h59m59s from scanner time.
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc.intelligence.htb
| Not valid before: 2021-04-19T00:43:16
|_Not valid after:  2022-04-19T00:43:16
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-06-03T22:02:31+00:00; +6h59m59s from scanner time.
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc.intelligence.htb
| Not valid before: 2021-04-19T00:43:16
|_Not valid after:  2022-04-19T00:43:16
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-06-03T22:02:33+00:00; +6h59m59s from scanner time.
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc.intelligence.htb
| Not valid before: 2021-04-19T00:43:16
|_Not valid after:  2022-04-19T00:43:16
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-06-03T22:02:31+00:00; +6h59m59s from scanner time.
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc.intelligence.htb
| Not valid before: 2021-04-19T00:43:16
|_Not valid after:  2022-04-19T00:43:16
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49683/tcp open  msrpc         Microsoft Windows RPC
49684/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49694/tcp open  msrpc         Microsoft Windows RPC
49741/tcp open  msrpc         Microsoft Windows RPC
54494/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: 6h59m58s, deviation: 0s, median: 6h59m58s
| smb2-time: 
|   date: 2024-06-03T22:01:53
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jun  3 11:02:34 2024 -- 1 IP address (1 host up) scanned in 147.25 seconds
```

### Enumeración del puerto 80 (http)
de manera inicial no vemos ninguna información que pueda ser útil en la pagina web
![[Pasted image 20240603112104.png]]
`Contacto`
en el apartado de contacto podemos ver un boton para descargar documentos PDF
![[Pasted image 20240603112212.png]]

### exiftool 
con esta herramienta podemos ver los metadatos de los documentos pdf donde podemos encontrar usuarios. me llama la atención en formato en el que están guardados estos documentos pdf
![[Pasted image 20240603114141.png]]

### Iterador for
dado que los documentos pdf están guardados de manera predeterminada con el año mes y día podemos crear un iterador for para que recorra una lista desde el año 2020 hasta el 2022 con todos los días y los meses de manera que podamos descargar cualquier archivo pdf que coincida con esta sintax
```python
for a in {2020..2022}; do for m in {1..12}; do for d in {1..31}; do echo "http://10.10.10.248/documents/$a-$m-$d-upload.pdf"; done; done; done | xargs -n 1 -P 20 wget
```
hemos descargado varios documentos pdf que ahora podemos analizar para obtener mas usuarios
![[Pasted image 20240603115218.png]]

`Obtenemos todos los usuarios`
de esta manera hacemos un exiftool para todos los documentos pdf y con el comando `grep` nos quedamos con la linea `Creator` y luego con el comando `awk` imprimimos la segunda linea solo donde destan los nombres de usuarios
![[Pasted image 20240603115931.png]]

### Kerbrute
utilizando kerbrute podemos hacer una validación de usuarios con la lista de usuarios que hemos conseguido 
![[Pasted image 20240603120831.png]]

### impacket-GetNPUsers 
intentamos realizar un `ASREPRoast` pero actualmente no podemos debido a que lo usuarios no cuentan con la autenticación de kerberos 
![[Pasted image 20240603134605.png]]

### pdftotext
esta herramienta nos permite convertir los archivos pdf a texto pero debido a que carece de un metodo que nos permita realizarlo a todos los archivos a la ves vamos a aplicar un iterador de nuevo
![[Pasted image 20240603135434.png]]

### Iterador
```python
for file in $(ls *pdf); do echo $file; done | grep -v users | while read filename; do pdftotext $filename; done
```
con este iterador podemos leer y hacer un pdftotext para cada archivo pdf que tenemos
![[Pasted image 20240603140449.png]]

`solo nos falta leer al documento que contenga algo de logica`
![[Pasted image 20240603140834.png]]

`una vez que encontrado el documento nos muestra la contraseña que se ha ingresado por default a cada usuario`
![[Pasted image 20240603162936.png]]

`otra forma de ver el documento`
básicamente nos están indicando que la contraseña por default del usuario es `NewIntelligenceCorpUser9876` asi que probaremos esto con los usuarios que ya tenemos
![[Pasted image 20240603163205.png]]

### crackmapexec 
hemos conseguido un usuario valido  así que vamos a realizar pruebas
![[Pasted image 20240603164054.png]]

### impacket-smbclient
ahora podemos enumerar los recursos compartidos de tiffany donde encontramos un archivo llamado downdetector.p
![[Pasted image 20240603171039.png]]