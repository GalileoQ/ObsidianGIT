### nmap
#Linux
```python
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 96:07:1c:c6:77:3e:07:a0:cc:6f:24:19:74:4d:57:0b (ECDSA)
|_  256 0b:a4:c0:cf:e2:3b:95:ae:f6:f5:df:7d:0c:88:d6:ce (ED25519)
80/tcp   open  http    Apache httpd 2.4.52
|_http-title: Did not follow redirect to http://codify.htb/
|_http-server-header: Apache/2.4.52 (Ubuntu)
2020/tcp open  http    Node.js Express framework
|_http-title: Error
3000/tcp open  http    Node.js Express framework
|_http-title: Codify
8083/tcp open  us-srv?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, Help, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, RPCCheck, SIPOptions, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServerCookie, X11Probe: 
|     HTTP/1.1 400 Bad Request
|     Connection: close
|   FourOhFourRequest: 
|     HTTP/1.1 200 OK
|     Content-Type: text/html
|     Date: Fri, 10 Nov 2023 02:48:29 GMT
|     Connection: close
|     <h2>Hello World!</h2>
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Content-Type: text/html
|     Date: Fri, 10 Nov 2023 02:48:28 GMT
|     Connection: close
|     <h2>Hello World!</h2>
|   HTTPOptions, RTSPRequest: 
|     HTTP/1.1 200 OK
|     Content-Type: text/html
|     Date: Fri, 10 Nov 2023 02:48:34 GMT
|     Connection: close
|_    <h2>Hello World!</h2>
8088/tcp open  http    SimpleHTTPServer 0.6 (Python 3.10.12)
|_http-title: Directory listing for /
|_http-server-header: SimpleHTTP/0.6 Python/3.10.12
```
### Ports: 22,80,2020,3000,8083,8088

### enumeramos la pagina web.
![[Pasted image 20231110200559.png]]
### tenemos un panel que es capas de interpretar ciertos codigos. en el apartado About us
![[Pasted image 20231110200743.png]]
el vm2 nos da informacion sobre la version 
![[Pasted image 20231110203956.png]]
tenemos un exploit de github que nos proporciona un codigo. usaremos esto para intentar una intrucion
![[Pasted image 20231110204410.png]]
usamos el id para que nos muestra informacion sobre el usuario y parece que funciona. usaremos esto para intentar tirar una reverse shell
### crearemos un archivo llamado index.html que contenta una reverse shell
![[Pasted image 20231110204727.png]]
### vamos a montar un servidor con python3 y estaremos en la escucha por el puerto que hemos configurado
![[Pasted image 20231110204928.png]]
### ahora haremos un curl para llamar este archivo
![[Pasted image 20231110205131.png]]
si todo va bien. deberiamos obtener una reverse shell por el puerto que hemos espeficado
![[Pasted image 20231110205254.png]]
### vemos que en la parte del servidor la peticion ha cido exitosa y en la terminal donde estabamos a la escucha hemos ganado una shell
![[Pasted image 20231110205751.png]]
### tenemos un usuario llamado joshua pero no podemos entrar a su directorio por lo que tendremos que hacer movimiento lateral
vamos al directorio /var/www/contact y tenemos un archivo llamado tickets.db
![[Pasted image 20231110211107.png]]
tenemos un hash para el usuario joshua 
![[Pasted image 20231110211756.png]]
usamos john para decibrar el hash
![[Pasted image 20231110211859.png]]
tenemos contraseña para el usuario joshua. entramos por ssh
![[Pasted image 20231110212823.png]]
### escalada de privilegios
podemos hacer sudo -l con joshua
![[Pasted image 20231110213954.png]]
### tenemos permisos sobre u archivo llamado msql-backup.sh
![[Pasted image 20231110214306.png]]
### al parecer este backup llama a la base de datos para sacar las credenciales de root. y tambien nos indica que si dejamos el password en blanco no deja password confirme

crearemos exploit en python para poder aprovecharnos de esto y capturar la contraseña
![[Pasted image 20231110214804.png]]
### con un servidor en python nos traemos este exploit a la maquina victima para poder usarlo
![[Pasted image 20231110215304.png]]
en este caso probaremos la ultima clave que se genera.
### WE ARE ROOT.
