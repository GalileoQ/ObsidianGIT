#Linux #Easy 
### nmap
```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 74:e0:e1:b4:05:85:6a:15:68:7e:16:da:f2:c7:6b:ee (RSA)
|   256 bd:43:62:b9:a1:86:51:36:f8:c7:df:f9:0f:63:8f:a3 (ECDSA)
|_  256 f9:e7:da:07:8f:10:af:97:0b:32:87:c9:32:d7:1b:76 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Smag
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 84.71 seconds
```

### Ports: 22/ssh - 80/http

### Enumeracion del puerto 80
![[Pasted image 20240110174447.png]]
###### parece que no tenemos mucha información debido a que la pagina esta en mantenimiento. y mirando el código fuente de la pagina tampoco podemos conseguir mucha información

### Fuzzing con gobuster
![[Pasted image 20240110175151.png]]
###### temos un subdominio llamado /mail asi que vamos a investigar 

### subdominio /mail
###### tenemos tres mensajes en esta dirección que nos proporcionan información valiosa
![[Pasted image 20240110175916.png]]
###### tenemos un archivo para descargar y 3 usuarios: NETADMIN, jAKE, UZI  

### Analizaremos el archivo .pcap con wireshark 
![[Pasted image 20240110183844.png]]
###### podemos ver un intercambio de conexiones entre dos ips diferentes asi que vamos a leer esta conexión para intentar extraer mas información 
![[Pasted image 20240110184616.png]]
###### descubrimos que este paquete contiene una conexión en un servicio login.php también tenemos un dominio llamado development.smag.thm y también tenemos unas credenciales. vamos a investigar el nuevo dominio para saber de que se trata

### agregamos el dominio al archivo /etc/hosts
![[Pasted image 20240110185219.png]]
###### una vez hecho esto podemos dirigirnos al dominio desde la direccion ip de la maquina
### obtenemos acceso al dominio en el cual nos encontramos con 3 dominios .php
![[Pasted image 20240110185504.png]]

### login.php
###### este parece ser el login.php en el cual se estaba realizando la peticion que hemos conseguido anteriormente. asi que vamos a probar las credenciales
![[Pasted image 20240110185746.png]]

### obtenemos conexion y esto nos lleva a un panel que nos permite ejecutar comando. usaremos esto para intentar conseguir una reverse shell
###### ingresamos un onliner que contenga una reverse shell. estaremos a la escucha por el puerto especificado
![[Pasted image 20240110190330.png]]
###### listening
![[Pasted image 20240110190620.png]]
### tareas crontab
![[Pasted image 20240110192841.png]]
###### tenemos una tarea /crontab que se ejecuta a cada minuto. esta tarea ejecuta un archivo que esta en la ruta /opt y lo guarda en el direcctorio de jake en la ruta .ssh/authorized_keys

### crearemos un par de claves ssh
![[Pasted image 20240110202942.png]]
### vamos a pegar la clave publica en el archivo jake_id_rsa.pub.backup. usaremos esto para entrar por el puerto ssh
![[Pasted image 20240110203643.png]]
##### tenemos conexion

### Escalada de privilegios
![[Pasted image 20240110203913.png]]
###### el usuario jake tiene permisos sudo para ejecutar el binario /apt/get usaremos esto para escalar privilegios
###### usaremos el codigo  "sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh" 
este codigo nos permite escalar privilegios y convertirnos en root
![[Pasted image 20240110204224.png]]
### WE ARE ROOT
