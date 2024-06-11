#Linux #medium 
### nmap 
```python
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 57:8a:da:90:ba:ed:3a:47:0c:05:a3:f7:a8:0a:8d:78 (RSA)
|   256 c2:64:ef:ab:b1:9a:1c:87:58:7c:4b:d5:0f:20:46:26 (ECDSA)
|_  256 5a:f2:62:92:11:8e:ad:8a:9b:23:82:2d:ad:53:bc:16 (ED25519)
80/tcp  open  http        Apache httpd 2.4.29 ((Ubuntu))
|_http-generator: WordPress 5.0
|_http-title: Billy Joel&#039;s IT Blog &#8211; The IT blog
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: BLOG; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: BLOG, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-time: 
|   date: 2024-01-20T02:40:28
|_  start_date: N/A
|_clock-skew: mean: -29s, deviation: 0s, median: -30s
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: blog
|   NetBIOS computer name: BLOG\x00
|   Domain name: \x00
|   FQDN: blog
|_  System time: 2024-01-20T02:40:28+00:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 56.03 seconds
```
### Ports: 22/ssh - 80/http - 139/ smb - 445/ smb

### Enumeración del puerto 80/http
###### CMS: WordPress 5.0 ; Users: Billy Joel
![[Pasted image 20240120202953.png]]
### Fuzzing con gobuster 

![[Pasted image 20240120202906.png]]
### Enumeracion con enum4linux
enumeración de recursos compartidos 
![[Pasted image 20240120200005.png]]

enumeración de usuarios
![[Pasted image 20240120201748.png]]

### Enumeracion de directorio http://blog.thm/wp-login.php
tenemos un `WordPress` podriamos intentar hacer un ataque de fuerza bruta utilizando `hydra o wp-scan`  

![[Pasted image 20240120210226.png]]

### Fuerza bruta con hydra
con un ataque de fuerza bruta utilizando hydra obtenemos un paquete de posibles contraseñas para intentar ganar acceso a la pagina

![[Pasted image 20240120210558.png]]
después de probar las contraseñas no obtuvimos acceso a la pagina así que seguiremos enumerando 

### Enumeracion con smbmap
en este caso tenemos un archivo compartido llamado `BillySMB` 
![[Pasted image 20240120211221.png]]

vamos a seguir la ruta del archivo
![[Pasted image 20240120212015.png]]

### Enumeración con wpscan

![[Pasted image 20240120213010.png]]
tenemos un recurso `robots.txt` que vamos a investigar 

tenemos un `uploads`
![[Pasted image 20240120214104.png]]

usuarios `bjoel, kwheel, karen wheeler, billy joel `
![[Pasted image 20240120213229.png]]

### robots.txt
![[Pasted image 20240120213838.png]]

### Ataque de fuerza bruta con wpscan - Usuario kheel
![[Pasted image 20240120220309.png]]

Obtenemos credenciales validas
![[Pasted image 20240120220415.png]]
### wp-admin
![[Pasted image 20240120220602.png]]
###### Comments
![[Pasted image 20240120220812.png]]
### msfconsole
usamos el exploit/multi/http/wp_crop_rce
![[Pasted image 20240120231023.png]]

show options
![[Pasted image 20240120231322.png]]
tenemos que setear: password, RHOSTS, USERNAME, LHOST

show options
![[Pasted image 20240120232025.png]]

run
![[Pasted image 20240120232752.png]]
tenemos conexión en la maquina

### Escalada de privilegio
el binario `/usr/bin/pkexec` es vulnerable asi que buscaremos un exploit que nos permita explotar esta ruta y ser usuario root
![[Pasted image 20240120232953.png]]

exploit en github para escalar privilegios 
![[Pasted image 20240120233550.png]]

creamos un servidor en python para compartirnos el exploit
![[Pasted image 20240120233508.png]]

ejecutamos el exploit siguiendo los paso
![[Pasted image 20240120233743.png]]

### WE ARE ROOT