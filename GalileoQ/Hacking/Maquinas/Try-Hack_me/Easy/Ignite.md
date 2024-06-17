#Linux #Easy 
### nmap
```python
sudo nmap -sCV 10.10.46.137
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-12 20:48 EST
Nmap scan report for 10.10.46.137
Host is up (0.21s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/fuel/
|_http-title: Welcome to FUEL CMS
|_http-server-header: Apache/2.4.18 (Ubuntu)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.78 seconds
```

### enumeración de pag web
![[Pasted image 20240612144324.png]]
##### tenemos el framework y la version 1.4
### gobuster
![[Pasted image 20240612144329.png]]
##### tenemos un archivo robots.txt

### revisando la pagina principal nos da informacion sobre las credenciales por defecto para el panel de administracion
![[Pasted image 20240612144334.png]]
usamos las credenciales
![[Pasted image 20240612144339.png]]

### buscamos en Internet y conseguimos un exploit que nos permite ejecución de datos
![[Pasted image 20240612144344.png]]

### ejecutamos. el exploit nos permite ejecutar comandos así que le lanzaremos una bash y estaremos a la escucha
![[Pasted image 20240612144349.png]]
### enumeramos todas las carpetas hasta llegar a la ruta /var/www/html/fuel/application 
![[Pasted image 20240612144354.png]]
### tenemos un archivo database.php
![[Pasted image 20240612144359.png]]
### tenemos credenciales para root.
![[Pasted image 20240612144404.png]]

### WE ARE ROOT