```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
|_  256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Follow the white rabbit.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 84.27 seconds
```

### Ports: 22/ssh - 80/http

### Enumeración del puerto 80
no tenemos mucha informacion en la pagina asi que vamos a realizar fuzzing
![[Pasted image 20240113203037.png]]

### Fuzzing con gobuster
tenemos algunos directorios que vamos a investigar 
![[Pasted image 20240113203227.png]]

### /img/
![[Pasted image 20240113203353.png]]

### /r/
![[Pasted image 20240113203444.png]]

### /poem/
![[Pasted image 20240113203709.png]]

### parece que el mas interesante es el directorio /r/ así que seguiremos investigando por aquí. haremos un nuevo ataque de fuzzing apuntando a este directorio
