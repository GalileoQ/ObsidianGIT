#Linux  #medium
### Ping
```python
ping -c 1 10.10.11.7
PING 10.10.11.7 (10.10.11.7) 56(84) bytes of data.
64 bytes from 10.10.11.7: icmp_seq=1 ttl=63 time=165 ms

--- 10.10.11.7 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 164.919/164.919/164.919/0.000 ms
```

### TTL 63=Linux
### nmap
```python
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
8080/tcp open  http-proxy Werkzeug/1.0.1 Python/2.7.18
| http-title: Site doesn't have a title (text/html; charset=utf-8).
|_Requested resource was http://10.10.11.7:8080/login
|_http-server-header: Werkzeug/1.0.1 Python/2.7.18
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 NOT FOUND
|     content-type: text/html; charset=utf-8
|     content-length: 232
|     vary: Cookie
|     set-cookie: session=eyJfcGVybWFuZW50Ijp0cnVlfQ.Zf8tOg.Rhf24FaJEMjmv8vzaosU_2H47as; Expires=Sat, 23-Mar-2024 19:32:54 GMT; HttpOnly; Path=/
|     server: Werkzeug/1.0.1 Python/2.7.18
|     date: Sat, 23 Mar 2024 19:27:54 GMT
|     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
|     <title>404 Not Found</title>
|     <h1>Not Found</h1>
|     <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
|   GetRequest: 
|     HTTP/1.0 302 FOUND
|     content-type: text/html; charset=utf-8
|     content-length: 219
|     location: http://0.0.0.0:8080/login
|     vary: Cookie
|     set-cookie: session=eyJfZnJlc2giOmZhbHNlLCJfcGVybWFuZW50Ijp0cnVlfQ.Zf8tOQ.2h60Jzy9hvv3faIHUPqWGwUZ9Q8; Expires=Sat, 23-Mar-2024 19:32:53 GMT; HttpOnly; Path=/
|     server: Werkzeug/1.0.1 Python/2.7.18
|     date: Sat, 23 Mar 2024 19:27:53 GMT
|     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
|     <title>Redirecting...</title>
|     <h1>Redirecting...</h1>
|     <p>You should be redirected automatically to target URL: <a href="/login">/login</a>. If not click the link.
|   HTTPOptions: 
|     HTTP/1.0 200 OK
|     content-type: text/html; charset=utf-8
|     allow: HEAD, OPTIONS, GET
|     vary: Cookie
|     set-cookie: session=eyJfcGVybWFuZW50Ijp0cnVlfQ.Zf8tOQ.wIfkmDPv4ZcV1LtFInvNk6ihU98; Expires=Sat, 23-Mar-2024 19:32:53 GMT; HttpOnly; Path=/
|     content-length: 0
|     server: Werkzeug/1.0.1 Python/2.7.18
|     date: Sat, 23 Mar 2024 19:27:53 GMT
|   RTSPRequest: 
|     HTTP/1.1 400 Bad request
|     content-length: 90
|     cache-control: no-cache
|     content-type: text/html
|     connection: close
|     <html><body><h1>400 Bad request</h1>
|     Your browser sent an invalid request.
|_    </body></html>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.94SVN%I=7%D=3/23%Time=65FF2D73%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,24C,"HTTP/1\.0\x20302\x20FOUND\r\ncontent-type:\x20text/htm
SF:l;\x20charset=utf-8\r\ncontent-length:\x20219\r\nlocation:\x20http://0\
SF:.0\.0\.0:8080/login\r\nvary:\x20Cookie\r\nset-cookie:\x20session=eyJfZn
SF:Jlc2giOmZhbHNlLCJfcGVybWFuZW50Ijp0cnVlfQ\.Zf8tOQ\.2h60Jzy9hvv3faIHUPqWG
SF:wUZ9Q8;\x20Expires=Sat,\x2023-Mar-2024\x2019:32:53\x20GMT;\x20HttpOnly;
SF:\x20Path=/\r\nserver:\x20Werkzeug/1\.0\.1\x20Python/2\.7\.18\r\ndate:\x
SF:20Sat,\x2023\x20Mar\x202024\x2019:27:53\x20GMT\r\n\r\n<!DOCTYPE\x20HTML
SF:\x20PUBLIC\x20\"-//W3C//DTD\x20HTML\x203\.2\x20Final//EN\">\n<title>Red
SF:irecting\.\.\.</title>\n<h1>Redirecting\.\.\.</h1>\n<p>You\x20should\x2
SF:0be\x20redirected\x20automatically\x20to\x20target\x20URL:\x20<a\x20hre
SF:f=\"/login\">/login</a>\.\x20\x20If\x20not\x20click\x20the\x20link\.")%
SF:r(HTTPOptions,14E,"HTTP/1\.0\x20200\x20OK\r\ncontent-type:\x20text/html
SF:;\x20charset=utf-8\r\nallow:\x20HEAD,\x20OPTIONS,\x20GET\r\nvary:\x20Co
SF:okie\r\nset-cookie:\x20session=eyJfcGVybWFuZW50Ijp0cnVlfQ\.Zf8tOQ\.wIfk
SF:mDPv4ZcV1LtFInvNk6ihU98;\x20Expires=Sat,\x2023-Mar-2024\x2019:32:53\x20
SF:GMT;\x20HttpOnly;\x20Path=/\r\ncontent-length:\x200\r\nserver:\x20Werkz
SF:eug/1\.0\.1\x20Python/2\.7\.18\r\ndate:\x20Sat,\x2023\x20Mar\x202024\x2
SF:019:27:53\x20GMT\r\n\r\n")%r(RTSPRequest,CF,"HTTP/1\.1\x20400\x20Bad\x2
SF:0request\r\ncontent-length:\x2090\r\ncache-control:\x20no-cache\r\ncont
SF:ent-type:\x20text/html\r\nconnection:\x20close\r\n\r\n<html><body><h1>4
SF:00\x20Bad\x20request</h1>\nYour\x20browser\x20sent\x20an\x20invalid\x20
SF:request\.\n</body></html>\n")%r(FourOhFourRequest,224,"HTTP/1\.0\x20404
SF:\x20NOT\x20FOUND\r\ncontent-type:\x20text/html;\x20charset=utf-8\r\ncon
SF:tent-length:\x20232\r\nvary:\x20Cookie\r\nset-cookie:\x20session=eyJfcG
SF:VybWFuZW50Ijp0cnVlfQ\.Zf8tOg\.Rhf24FaJEMjmv8vzaosU_2H47as;\x20Expires=S
SF:at,\x2023-Mar-2024\x2019:32:54\x20GMT;\x20HttpOnly;\x20Path=/\r\nserver
SF::\x20Werkzeug/1\.0\.1\x20Python/2\.7\.18\r\ndate:\x20Sat,\x2023\x20Mar\
SF:x202024\x2019:27:54\x20GMT\r\n\r\n<!DOCTYPE\x20HTML\x20PUBLIC\x20\"-//W
SF:3C//DTD\x20HTML\x203\.2\x20Final//EN\">\n<title>404\x20Not\x20Found</ti
SF:tle>\n<h1>Not\x20Found</h1>\n<p>The\x20requested\x20URL\x20was\x20not\x
SF:20found\x20on\x20the\x20server\.\x20If\x20you\x20entered\x20the\x20URL\
SF:x20manually\x20please\x20check\x20your\x20spelling\x20and\x20try\x20aga
SF:in\.</p>\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 43.52 seconds
```

### Enumeración del puerto 8080(http)
tenemos un CMS openplc
![[Pasted image 20240323153128.png]]

### searchsploit
conseguimos un `Remote code execution`
![[Pasted image 20240323161500.png]]

`default credential`
![[Pasted image 20240323153432.png]]

### OpenPLC

![[Pasted image 20240323153455.png]]

`parece que el programa lleva extenciones .st las cuales son interpretaciones del codigo C`
![[Pasted image 20240323162615.png]]

`Reverse shell en lenguaje C`
![[Pasted image 20240323162544.png]]

`En Hardware tenemos la pestaña de Hardware Layer box. aqui podemos intentar inyectamos la reverse shell en lenguaje C al final de las sentencias`
en esta parte incluimos las librerías de nuestra revershe shell
![[Pasted image 20240323164928.png]]

`en la parte final podemos declarar el resto de nuestra reverse shell`
![[Pasted image 20240323165027.png]]

`guardamos los cambios, parece que todo va perfecto y vamos al Go To Dashboard` 
![[Pasted image 20240323165131.png]]

### nc -lvnp 
estaremos a la escucha e iniciamos el servicio `Star PLC` de esta manera obtenemos una conexión 
![[Pasted image 20240323165523.png]]

### TTY
una ves conseguida la intrusión vamos a estabilizar nuestra TTY en este caso lo haremos con python3 ya que la maquina tiene este servicio
`Nota: actualmente somos root pero creo que estamos dentro de un contenedor`
![[Pasted image 20240323170148.png]]

### sqlite3
tenemos un archivo openplc.db el cual podemos ver con sqlite3.
![[Pasted image 20240323171004.png]]

### linpeas.sh
vamos a subir el linpeas para explorar maneras de escapar del contenedor
![[Pasted image 20240323172722.png]]

`èjecutamos el linpeas`
![[Pasted image 20240323173306.png]]

`obtenemos una posible via de ataque con release_agent breakout 2`
![[Pasted image 20240323173358.png]]

`Interfaz de red`
tambien tenemos una interfaz de red wifi que parece estar en modo monitor
![[Pasted image 20240323173700.png]]

