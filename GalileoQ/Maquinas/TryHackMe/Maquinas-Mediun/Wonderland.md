```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
|_  256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Follow the white rabbit.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 84.27 seconds
```

### Ports: 22/ssh - 80/http

### Enumeración del puerto 80
no tenemos mucha informacion en la pagina asi que vamos a realizar fuzzing
![[Pasted image 20240113203037.png]]

### Fuzzing con gobuster
tenemos algunos directorios que vamos a investigar 
![[Pasted image 20240113203227.png]]

### /img/
![[Pasted image 20240113203353.png]]

### /r/
![[Pasted image 20240113203444.png]]

### /poem/
![[Pasted image 20240113203709.png]]

### parece que el mas interesante es el directorio /r/ así que seguiremos investigando por aquí. haremos un nuevo ataque de fuzzing apuntando a este directorio
![[Pasted image 20240113203946.png]]

### tenemos un nuevo directorio que agregaremos a la ruta y nos quedaria de la siguiente manera http://10.10.63.231/r/a/
![[Pasted image 20240113204104.png]]
tenemos el mismo mensaje asi que seguiremos haciendo fuzzing

### la ruta rotal nos lleva al directorio http://10.10.63.231/r/a/b/b/i/t/
![[Pasted image 20240113204522.png]]

### miramos el código fuente y conseguimos información que parece ser credenciales
![[Pasted image 20240113204827.png]]

### extraemos informacion de las imagenes 
una de las imagenes contiene un archivo.txt pero no es de mucha informacion
![[Pasted image 20240113205534.png]]

### Port 22/ssh
vamos a intentar ingresar al puerto ssh con la inofrmacion encontrada en el codigo fuente de la pagina web
![[Pasted image 20240113205819.png]]
tenemos conexion como el usuario alice

### Escalada de privilegios 
![[Pasted image 20240113210239.png]]
el usuario rabbit tiene permiso para ejecutar el siguiente binario
### el archivo importa la libreria ramdon de python
![[Pasted image 20240113212411.png]]
usaremos esto para realizar una especia de hijacking. basicamente tenemos la ruta donde se esta ejecutando el script asi que si creamos un archivo llamado random el script llamara primero a este archivo antes que a la libreria 
### primero crearemos un archivo llamado random.py
![[Pasted image 20240113213035.png]]
