#Advanced-Labs 

### Ping

```python
ping -c 1 10.13.37.14
PING 10.13.37.14 (10.13.37.14) 56(84) bytes of data.
64 bytes from 10.13.37.14: icmp_seq=1 ttl=63 time=167 ms

--- 10.13.37.14 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 166.552/166.552/166.552/0.000 ms
```

### nmap

```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-12 17:20 EDT
Nmap scan report for 10.13.37.14
Host is up (0.17s latency).
Not shown: 65524 closed tcp ports (reset), 8 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE         VERSION
22/tcp   open  ssh             OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 a8:05:53:ae:b1:8d:7e:90:f1:ea:81:6b:18:f6:5a:68 (RSA)
|   256 2e:7f:96:ec:c9:35:df:0a:cb:63:73:26:7c:15:9d:f5 (ECDSA)
|_  256 2f:ab:d4:f5:48:45:10:d2:3c:4e:55:ce:82:9e:22:3a (ED25519)
80/tcp   open  http            nginx 1.13.12
| http-title: Notifications
|_Requested resource was http://10.13.37.14/login?next=%2F
| http-git: 
|   10.13.37.14:80/.git/
|     Git repository found!
|     .git/config matched patterns 'user'
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: Add app logic & requirements.txt 
|_http-server-header: nginx/1.13.12
8888/tcp open  sun-answerbook?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, JavaRMI, Kerberos, LDAPBindReq, LDAPSearchReq, LPDString, LSCP, RPCCheck, RTSPRequest, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServerCookie, X11Probe: 
|     Welcome to FaradaySEC stats!!!
|     Username: Bad chars detected!
|   NULL: 
|     Welcome to FaradaySEC stats!!!
|_    Username:
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8888-TCP:V=7.94SVN%I=7%D=7/12%Time=66919E43%P=x86_64-pc-linux-gnu%r
SF:(NULL,29,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20")%r(Ge
SF:tRequest,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\
SF:x20chars\x20detected!")%r(HTTPOptions,3C,"Welcome\x20to\x20FaradaySEC\x
SF:20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(FourOhFourReques
SF:t,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20char
SF:s\x20detected!")%r(JavaRMI,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\
SF:nUsername:\x20Bad\x20chars\x20detected!")%r(LSCP,3C,"Welcome\x20to\x20F
SF:aradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(Gener
SF:icLines,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x
SF:20chars\x20detected!")%r(RTSPRequest,3C,"Welcome\x20to\x20FaradaySEC\x2
SF:0stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(RPCCheck,3C,"Welc
SF:ome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detec
SF:ted!")%r(DNSVersionBindReqTCP,3C,"Welcome\x20to\x20FaradaySEC\x20stats!
SF:!!\nUsername:\x20Bad\x20chars\x20detected!")%r(DNSStatusRequestTCP,3C,"
SF:Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20d
SF:etected!")%r(Help,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername
SF::\x20Bad\x20chars\x20detected!")%r(SSLSessionReq,3C,"Welcome\x20to\x20F
SF:aradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(Termi
SF:nalServerCookie,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\
SF:x20Bad\x20chars\x20detected!")%r(TLSSessionReq,3C,"Welcome\x20to\x20Far
SF:adaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(Kerbero
SF:s,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20char
SF:s\x20detected!")%r(SMBProgNeg,3C,"Welcome\x20to\x20FaradaySEC\x20stats!
SF:!!\nUsername:\x20Bad\x20chars\x20detected!")%r(X11Probe,3C,"Welcome\x20
SF:to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%
SF:r(LPDString,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20B
SF:ad\x20chars\x20detected!")%r(LDAPSearchReq,3C,"Welcome\x20to\x20Faraday
SF:SEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(LDAPBindReq
SF:,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars
SF:\x20detected!");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.88 seconds
```

### Enumeración del puerto 80 (http)
parece que aqui podemos crear una cuenta para iniciar sesión 
![[Pasted image 20240712172822.png]]

`Alert system`
creamos la cuenta y luego podemos iniciar sesión
![[Pasted image 20240712172959.png]]

`Alert system`
aqui nos solicita una configuración para el servidor SMTP que recibirá alertas del sistema; Configuramos nuestro host con el puerto 25
![[Pasted image 20240712173218.png]]

seleccionamos un nombre para el servidor. en este caso lo dejo por defecto y le damos en `Send`
![[Pasted image 20240712174048.png]]

agregamos los datos que queremos para la alerta en este caso usare solo `test`
![[Pasted image 20240712174121.png]]

`flag`
despues de enviar la alerta la recibimos en nuestro servidor de python en donde podemos ber la primera 
![[Pasted image 20240712173809.png]]














### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
