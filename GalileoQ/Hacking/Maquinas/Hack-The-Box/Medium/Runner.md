#fuzzing #Linux #medium #nmap #tecnicas 
### Ping
```python
ping -c 1 10.10.11.13
PING 10.10.11.13 (10.10.11.13) 56(84) bytes of data.
64 bytes from 10.10.11.13: icmp_seq=1 ttl=63 time=76.3 ms

--- 10.10.11.13 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 76.279/76.279/76.279/0.000 ms
```

### TTL 63 = Linux

### nmap
```python
sudo nmap -p- --open -sC -sV --min-rate 3000 -n -Pn 10.10.11.13 -oN Scan
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-30 10:36 EDT
RTTVAR has grown to over 2.3 seconds, decreasing to 2.0
RTTVAR has grown to over 2.3 seconds, decreasing to 2.0
Nmap scan report for 10.10.11.13
Host is up (0.072s latency).
Not shown: 53707 closed tcp ports (reset), 11825 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp   open  http        nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://runner.htb/
8000/tcp open  nagios-nsca Nagios NSCA
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.30 seconds
```

### Enumeración del puerto 80 (http)
por el momento no vemos nada interesante. vamos a enumerar el puerto 8000
![[Pasted image 20240612122922.png]]

### Enumeración del puerto 8000 (http)
parece no poder encontrar el dominio. podemos hacer fuzzing para investigar mas a fondo
![[Pasted image 20240612122928.png]]

### Fuzzing con ffuzz (Directorios)
no encontramos gran cosa en la enumeración de directorios así que vamos a enumerar subdominios 
![[Pasted image 20240612122933.png]]

`health`
este dominio nos estraga un `ok`
![[Pasted image 20240619203434.png]]
### Fuzzing con wfuzz (subtominios)
tenemos un subdominio algo interesante. 
![[Pasted image 20240612122939.png]]

`este subdominio nos lleva a una pagina de login en el cual podemos ver la versión del CMS`
![[Pasted image 20240612122944.png]]

### Exploit-DB
conseguimos una CVE para esta versión que crea un nuevo token y un usuario administrador para usted y todo lo que necesita es la URL de interés.
![[Pasted image 20240612122949.png]]

### ejecutamos el exploit
obtenemos un Username y una Password. además también obtenemos el token de session
![[Pasted image 20240612122954.png]]

### obtenemos acceso a la pagina
ahora solo debemos conseguir la manera de obtener una reverse shell
![[Pasted image 20240612123002.png]]

`User Management`
podemos encontrar 2 usuarios (John y Matthew)
![[Pasted image 20240627140241.png]]

`Backup`
también podemos ver un apartado para iniciar una copia de seguridad. podemos descargarlo y analizarla en local
![[Pasted image 20240627140457.png]]

`backup`
tenemos un archivo users que contiene 2 hash
![[Pasted image 20240627142921.png]]

`id_rsa`
también conseguimos un id_rsa
![[Pasted image 20240627165145.png]]

`ssh`
iniciamos sesión con alguno de los usuarios que conseguimos anteriormente t obtenemos acceso
![[Pasted image 20240627165041.png]]

### Enumeración de puertos internos

```python
	ss -tunlp | grep '127.0.0.1' --color=auto
```

podemos observar que se están ejecutando los puertos `8111-9493-9000-5005` 
![[Pasted image 20240627170133.png]]

### ssh port forwarding
vamos a traernos el puerto `9000` de la maquina victima al puerto `9000` de la maquina atacante para poder verlo en local usando ssh

```python
	 ssh -i id_rsa john@10.10.11.13 -L 9000:127.0.0.1:9000
```

![[Pasted image 20240627170637.png]]

### web forwarding
podemos obtener acceso al servicio que esta corriendo en el puerto `9000` que en este caso es un `http` con una `CMS portainer.io` 

`#Nota:podemos ingresar a la web usando las credenciales que Matthew las cuales consegimos despues de desifrar el hash | Matthew:piper123`
![[Pasted image 20240627171108.png]]

`portainer.io`
tenemos la versión del CMS
![[Pasted image 20240627172903.png]]

`volumens`
hacemos clic en el contenedor `primary` luego vamos a `volumes` y luego vamos a `Add volume`
![[Pasted image 20240627183840.png]]

`volumes`
creamos un volumen que este asociado a el dispositivo principal en la raíz del sistema. se debe ver de esta forma
![[Pasted image 20240627184401.png]]

``volumens
ya tenemos el volumen creado así que solo nos falta crear un contenedor. 
![[Pasted image 20240627184421.png]]

`Container`
ahora crearemos el contenedor haciendo clic en `Containers >> Add container`
![[Pasted image 20240627184540.png]]

`container`
en la primera parte debemos colocar el nombre del container en este caso se llamara `pwny` y en la imagen debemos indicarle una imagen que en este caso será la imagen de `Ubuntu:lates` `(Siguiente imagen)`
![[Pasted image 20240627184843.png]]

`Imagenes`
aquí podemos hacer clic en la imagen y copiar el id completo de la imagen `sha256:ca.........................................`
![[Pasted image 20240627184809.png]]

`Advanced container settings` 
aquí solo vamos a indicar que queremos tener una consola `interactive & TTY`
![[Pasted image 20240627185134.png]]

`Volumes`
finalmente especificamos el volumen y la ruta para montar y le damos a `Deploy the container`
![[Pasted image 20240627185437.png]]

`console`
una ves tenemos creado el contenedor solo debemos ir a la parte de `console` para
![[Pasted image 20240627185649.png]]