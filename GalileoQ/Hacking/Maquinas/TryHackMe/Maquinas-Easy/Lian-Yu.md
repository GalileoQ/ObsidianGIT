#Linux #Easy 
### nmap
```python
PORT      STATE SERVICE REASON         VERSION
21/tcp    open  ftp     syn-ack ttl 63 vsftpd 3.0.2
22/tcp    open  ssh     syn-ack ttl 63 OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
| ssh-hostkey: 
|   1024 56:50:bd:11:ef:d4:ac:56:32:c3:ee:73:3e:de:87:f4 (DSA)
| ssh-dss AAAAB3NzaC1kc3MAAACBAOZ67Cx0AtDwHfVa7iZw6O6htGa3GHwfRFSIUYW64PLpGRAdQ734COrod9T+pyjAdKscqLbUAM7xhSFpHFFGM7NuOwV+d35X8CTUM882eJX+t3vhEg9d7ckCzNuPnQSpeUpLuistGpaP0HqWTYjEncvDC0XMYByf7gbqWWU2pe9HAAAAFQDWZIJ944u1Lf3PqYCVsW48Gm9qCQAAAIBfWJeKF4FWRqZzPzquCMl6Zs/y8od6NhVfJyWfi8APYVzR0FR05YCdS2OY4C54/tI5s6i4Tfpah2k+fnkLzX74fONcAEqseZDOffn5bxS+nJtCWpahpMdkDzz692P6ffDjlSDLNAPn0mrJuUxBFw52Rv+hNBPR7SKclKOiZ86HnQAAAIAfWtiPHue0Q0J7pZbLeO8wZ9XNoxgSEPSNeTNixRorlfZBdclDDJcNfYkLXyvQEKq08S1rZ6eTqeWOD4zGLq9i1A+HxIfuxwoYp0zPodj3Hz0WwsIB2UzpyO4O0HiU6rvQbWnKmUaH2HbGtqJhYuPr76XxZtwK4qAeFKwyo87kzg==
|   2048 39:6f:3a:9c:b6:2d:ad:0c:d8:6d:be:77:13:07:25:d6 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRbgwcqyXJ24ulmT32kAKmPww+oXR6ZxoLeKrtdmyoRfhPTpCXdocoj0SqjsETI8H0pR0OVDQDMP6lnrL8zj2u1yFdp5/bDtgOnzfd+70Rul+G7Ch0uzextmZh7756/VrqKn+rdEVWTqqRkoUmI0T4eWxrOdN2vzERcvobqKP7BDUm/YiietIEK4VmRM84k9ebCyP67d7PSRCGVHS218Z56Z+EfuCAfvMe0hxtrbHlb+VYr1ACjUmGIPHyNeDf2430rgu5KdoeVrykrbn8J64c5wRZST7IHWoygv5j9ini+VzDhXal1H7l/HkQJKw9NSUJXOtLjWKlU4l+/xEkXPxZ
|   256 a6:69:96:d7:6d:61:27:96:7e:bb:9f:83:60:1b:52:12 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPfrP3xY5XGfIk2+e/xpHMTfLRyEjlDPMbA5FLuasDzVbI91sFHWxwY6fRD53n1eRITPYS1J6cBf+QRtxvjnqRg=
|   256 3f:43:76:75:a8:5a:a6:cd:33:b0:66:42:04:91:fe:a0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDexCVa97Otgeg9fCD4RSvrNyB8JhRKfzBrzUMe3E/Fn
80/tcp    open  http    syn-ack ttl 63 Apache httpd
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Purgatory
|_http-server-header: Apache
111/tcp   open  rpcbind syn-ack ttl 63 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          42966/tcp6  status
|   100024  1          43871/udp6  status
|   100024  1          55726/udp   status
|_  100024  1          57789/tcp   status
57789/tcp open  status  syn-ack ttl 63 1 (RPC #100024)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:34
Completed NSE at 21:34, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:34
Completed NSE at 21:34, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:34
Completed NSE at 21:34, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 103.54 seconds
           Raw packets sent: 80311 (3.534MB) | Rcvd: 80070 (3.281MB)
```

### Ports: 21/ftp - 22/ssh - 80/http - 111/tcp - 57789/tcp

## Enumeración del puerto 80
![[Pasted image 20231202223932.png]]
no parece haber mucha cosa en la pagina web y el codigo fuente parece estar bien.

### Fuzzing con wfuzz
![[Pasted image 20231202225935.png]]

subdominio /island
![[Pasted image 20231202225848.png]]

subdominio /island/2100
![[Pasted image 20231202230524.png]]

### codigo fuente del subdominio /island/2100
![[Pasted image 20231202230745.png]]
tenemos un mensaje que nos deja saber que tenemos una pista que lleva como agregado .ticket
### Fuzzing con wfuzz
![[Pasted image 20231202234008.png]]
tenemos el subdominio green_arrow y a esto le agregamos uestra pista .ticket

### subdominio http://IP/island/2100/green_arrow.ticket
![[Pasted image 20231202234220.png]]

### Hemos desencriptado el codigo y tenemos la contraseña asi que intentaremos las crendenciales para ssh y para ftp
### tenemos acceso por ftp 
```python

user: vigilante
pass: !#th3h00d

─────────────────────────────────

> echo "RTy8yhBQdscX" | base58 -d
!#th3h00d                        
```
![[Pasted image 20231203001816.png]]

### tenemos 3 archivos en el servicio ftp asi que vamos a descargarlos todos
![[Pasted image 20231203002714.png]]

### las imagenes parecen estar codificadas y una de ellas tiene el hexadecimal dañado asi que lo vamos a reparar 
![[Pasted image 20231203010904.png]]

logramos reparar la image y nos ha proporcionado una password que nos ha funcionado para decifrar otra imagen la cual tenia un archivo ss.zip

### despues de investigar en google conseguimos el nombre de la persona de la imagen
![[Pasted image 20231203014822.png]]

### ya tenemos credenciales asi que intentaremos entrar por ssh
![[Pasted image 20231203015004.png]]

### Podemos ejecutar pkexec como root asi que esto sera facil para escalar privilegios
![[Pasted image 20231203015735.png]]

### WE ARE ROOT