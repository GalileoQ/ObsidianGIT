#
### Ping
```python
ping -c 1 10.10.11.11
PING 10.10.11.11 (10.10.11.11) 56(84) bytes of data.
64 bytes from 10.10.11.11: icmp_seq=1 ttl=63 time=147 ms

--- 10.10.11.11 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 147.400/147.400/147.400/0.000 ms
```

### TTL 63=Linux

### nmap
```python
# Nmap 7.94SVN scan initiated Sat May 25 22:29:36 2024 as: nmap -p- --open -sC -sV --min-rate 3000 -n -Pn -oN Scan 10.10.11.11
Nmap scan report for 10.10.11.11
Host is up (0.15s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 06:2d:3b:85:10:59:ff:73:66:27:7f:0e:ae:03:ea:f4 (RSA)
|   256 59:03:dc:52:87:3a:35:99:34:44:74:33:78:31:35:fb (ECDSA)
|_  256 ab:13:38:e4:3e:e0:24:b4:69:38:a9:63:82:38:dd:f4 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat May 25 22:30:15 2024 -- 1 IP address (1 host up) scanned in 39.22 seconds
```

### Enumeración del Puerto 80 (http)

![[Pasted image 20240525223335.png]]

`Tenemos un panel request que podriamos intentar explotar`

![[Pasted image 20240525223730.png]]

### Enumeración con wfuzz
al enumerar subdominios podemos encontrar uno
![[Pasted image 20240526155349.png]]

`Login`
tenemos un panel de login al que intentaremos acceder buscando las credenciales por defecto y podemos ver la versión de la web
![[Pasted image 20240526155447.png]]

`credenciales por defecto`
![[Pasted image 20240526155556.png]]

### CVE-2023-30253
tenemos una cve que nos permite conectar una reverse shell a nuestra maquina
![[Pasted image 20240528104710.png]]

`Reverse shell`
ejecutando este exploits podemos obtener una conexión 
![[Pasted image 20240528104539.png]]

### User Pivoting
actualmente somos www-data. y tenemos otro usuario asi que debemos hacer un m
![[Pasted image 20240528115819.png]]


`el sub dominio crm pertenece a una base de datos asi que mirando la configuracion del archivo podemos ver un par de credenciales`
![[Pasted image 20240528115553.png]]