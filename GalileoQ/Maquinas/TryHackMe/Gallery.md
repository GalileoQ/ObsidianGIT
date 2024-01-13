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
![[Pasted image 20240112211850.png]]
la rever shell ha funcionado tenemos conexion a la maquina

### 