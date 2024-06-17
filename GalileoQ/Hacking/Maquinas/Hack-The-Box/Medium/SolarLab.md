#ActiveDirectory #medium #tecnicas #windows
### Ping
```python
ping -c 1 10.10.11.16
PING 10.10.11.16 (10.10.11.16) 56(84) bytes of data.
64 bytes from 10.10.11.16: icmp_seq=1 ttl=127 time=151 ms

--- 10.10.11.16 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 150.560/150.560/150.560/0.000 ms
```

### TTL 127= windows

### nmap
```python
Nmap scan report for 10.10.11.16
Host is up (0.17s latency).
Not shown: 65530 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE       VERSION
80/tcp   open  http          nginx 1.24.0
|_http-server-header: nginx/1.24.0
|_http-title: Did not follow redirect to http://solarlab.htb/
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
6791/tcp open  http          nginx 1.24.0
|_http-server-header: nginx/1.24.0
|_http-title: Did not follow redirect to http://report.solarlab.htb:6791/
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1m27s
| smb2-time: 
|   date: 2024-05-14T18:58:02
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 124.66 seconds
```

### Enumeración del puerto 80

![[Pasted image 20240612132552.png]]

### SMB
logramos establecer una coneccion por `SMB` y en el recurso compartido Documents obtenemos un archivo llamado `details-file.xlsx`
![[Pasted image 20240612132558.png]]

`libreoffice`
al abrir el archivo obtenemos credenciales de usuarios y contraseñas 
![[Pasted image 20240612132603.png]]

### Crackmapexec
creamos un archivo que contenga los usuarios y uno que contenga las contraseñas y validamos usuarios. 
![[Pasted image 20240612132608.png]]

### Enumeración del puerto 80 (http) Port:6791
tenemos un login y un montón de usuarios y contraseñas que podemos usar para intentar iniciar sesión
`Nota: tenemos que hacer una pequeña lista de usuarios en la cual agregamos la inicial del segundo nombre de los usuarios al inicio y al final del nombre. esto nos ha resultado en que el usuario blaker realmente es blakern`
![[Pasted image 20240612132614.png]]

### ReportHub
con las credenciales que hemos conseguido logramos tener acceso a la pagina de login
![[Pasted image 20240612132619.png]]

### CVE 2023-33733
después de hacer un montón de pruebas hemos encontrado una CVE que funciona. solo debemos hacer un git clone del repo e instalar los requirements para poder ejecutar esta prueba
![[Pasted image 20240612132624.png]]

`Traning request`
![[Pasted image 20240612132629.png]]

### Burpsuite
interceptamos la petición con burpsuite y usando este comando y cambiando el `touch /tmp/exploited` por una reverse shell de powareshell en base64 logramos establecer una conexión
![[Pasted image 20240612132634.png]]
### Estaremos a la escucha
obtenemos una reverse shell en Powareshell 
![[Pasted image 20240612132639.png]]

### Escalada de Privilegio
despues de indagar un poco hemos encontrado una base de datos que vamos a descargar
![[Pasted image 20240612132643.png]]

### sqlite3
obtenemos la base de datos con usuarios y contraseñas
![[Pasted image 20240612132652.png]]

### Port-Forwarding
vamos a crear un servidor con chisel en nuestra maquina atacante
![[Pasted image 20240612132657.png]]

`Maquina victima`
de esta manera hemos redireccionado toda la información que esta pasando por el puerto 9090 y 9091 al 127.0.0.1:8081
![[Pasted image 20240612132701.png]]

### Port-Forwarding 
obteniendo acceso a la pagina web que esta corriendo en el puerto 9090 de la maquina victima
![[Pasted image 20240612132705.png]]

### CVE 2023-32315

![[Pasted image 20240612132711.png]]

###### obtenemos acceso con las credenciales que hemos generado
![[Pasted image 20240612132715.png]]

### msfconsole
usaremos este exploit para establecer una conexión 
![[Pasted image 20240612132719.png]]

`seteamos la configuraxión`
![[Pasted image 20240612132726.png]]


`obtenemos conexión`
![[Pasted image 20240612132733.png]]

### openfire.script
parece que es un script que guarda la configuración de la pagina web
![[Pasted image 20240612132738.png]]

`passwords`
![[Pasted image 20240612132742.png]]

### openfire_decrypt
de esta manera utilizando la clave encriptada de administrador y la salt podemos desencriptar la clave
![[Pasted image 20240612132747.png]]

### msfvenom
creamos un payload que nos permita conectarnos a la maquina y con un servidor en python lo compartimos con la maquina victima
![[Pasted image 20240612132752.png]]

`descargamos el payload en la maquina victima`
![[Pasted image 20240612132758.png]]

### /multi/handler
vamos a setear el multi/handler
![[Pasted image 20240612132804.png]]

en la maquina victima podemos correr el comando de runas para invocar una reverse shellcon las credenciales de administrador
```python
.\RunasCs.exe Administrator ThisPasswordShouldDo!@ root.exe
```

![[Pasted image 20240612132809.png]]

`Recibimos conexión en el multi/handler`

![[Pasted image 20240612132814.png]]

### WE ARE ROOT