#Linux #medium 
### nmap
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
![[Pasted image 20240612154551.png]]

### Fuzzing con gobuster
tenemos algunos directorios que vamos a investigar 
![[Pasted image 20240612154555.png]]

### /img/
![[Pasted image 20240612154558.png]]

### /r/
![[Pasted image 20240612154601.png]]

### /poem/
![[Pasted image 20240612154607.png]]

### parece que el mas interesante es el directorio /r/ así que seguiremos investigando por aquí. haremos un nuevo ataque de fuzzing apuntando a este directorio
![[Pasted image 20240612154612.png]]

### tenemos un nuevo directorio que agregaremos a la ruta y nos quedaria de la siguiente manera http://10.10.63.231/r/a/
![[Pasted image 20240612154617.png]]
tenemos el mismo mensaje asi que seguiremos haciendo fuzzing

### la ruta rotal nos lleva al directorio http://10.10.63.231/r/a/b/b/i/t/
![[Pasted image 20240612154624.png]]

### miramos el código fuente y conseguimos información que parece ser credenciales
![[Pasted image 20240612154628.png]]

### extraemos informacion de las imagenes 
una de las imagenes contiene un archivo.txt pero no es de mucha informacion
![[Pasted image 20240612154632.png]]

### Port 22/ssh
vamos a intentar ingresar al puerto ssh con la inofrmacion encontrada en el codigo fuente de la pagina web
![[Pasted image 20240612154637.png]]
tenemos conexion como el usuario alice

### Escalada de privilegios 
![[Pasted image 20240612154640.png]]
el usuario rabbit tiene permiso para ejecutar el siguiente binario
### el archivo importa la libreria ramdon de python
![[Pasted image 20240612154646.png]]
usaremos esto para realizar una especia de hijacking. basicamente tenemos la ruta donde se esta ejecutando el script asi que si creamos un archivo llamado random el script llamara primero a este archivo antes que a la libreria 
### primero crearemos un archivo llamado random.py
![[Pasted image 20240612154653.png]]

### ejecutaremos el script para cambiar de usuario

```python
sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
```

![[Pasted image 20240612154700.png]]
logramos cambiar de usuario a rabbit

### vamos al directorio del usuario rabbit
![[Pasted image 20240612154704.png]]
tenemos otro 

### PATH hijacking 
![[Pasted image 20240612154708.png]]
el escript utiliza la variable date de linux asi que podemos utilizar esto para crear un archivo llamado date y poder agregarlo en el path

### creamos el archivo date 
le damos permisos de lectura al archivo
![[Pasted image 20240612154714.png]]
### agregamos date al PATH
![[Pasted image 20240612154718.png]]

### ejecutamos el script 

![[Pasted image 20240612154722.png]]
hemos cambiado de usuario exitosamente

### Escalada de privilegios
enumeracion de capabilitis
![[Pasted image 20240612154728.png]]
tenemos el binario perl el cual usaremos para escalar privilegios

```python
perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'
```

![[Pasted image 20240612154733.png]]

### WE ARE ROOT
