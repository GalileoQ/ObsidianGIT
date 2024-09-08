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

### Enumeración de usuarios

Teniendo en cuenta lo que se encontró anteriormente podremos enumerar los usuarios. en el numero 1 tenemos a `administra`

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