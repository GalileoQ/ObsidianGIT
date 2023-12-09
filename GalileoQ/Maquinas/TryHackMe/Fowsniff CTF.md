### nmap
```python
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 90:35:66:f4:c6:d2:95:12:1b:e8:cd:de:aa:4e:03:23 (RSA)
|   256 53:9d:23:67:34:cf:0a:d5:5a:9a:11:74:bd:fd:de:71 (ECDSA)
|_  256 a2:8f:db:ae:9e:3d:c9:e6:a9:ca:03:b1:d7:1b:66:83 (ED25519)
80/tcp  open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Fowsniff Corp - Delivering Solutions
|_http-server-header: Apache/2.4.18 (Ubuntu)
110/tcp open  pop3    Dovecot pop3d
|_pop3-capabilities: TOP RESP-CODES UIDL SASL(PLAIN) PIPELINING AUTH-RESP-CODE CAPA USER
143/tcp open  imap    Dovecot imapd
|_imap-capabilities: Pre-login AUTH=PLAINA0001 more have IDLE post-login listed OK capabilities SASL-IR IMAP4rev1 LITERAL+ LOGIN-REFERRALS ID ENABLE
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.59 seconds
```
### Ports: 22/ssh - 80/hhtp - 110/pop3 143/imap

### Enumeracion del puerto 80
![[Pasted image 20231207233234.png]]
tenemos un mensaje que nos da informacion sobre un hackeo que ha sufrido la empresa. parece que secuestraron su cuenta de twitter. vamos a mirar
### Twitter
![[Pasted image 20231207233456.png]]
vamos a mirar el link
### Informacion sobre el hackeo
![[Pasted image 20231207233636.png]]

### backups links
![[Pasted image 20231207233937.png]]
tenemos algunos correos electronicos con lo que parece ser el hash
### Fuzzing con gobuster
![[Pasted image 20231207231157.png]]
parece que los directorios no tienen nada que sea demasiado importante asi que vamos a decifrar los correos electronicos y los hashes
### usaremos hashid para identificar el tipo de hash de cada uno de los correos electronicos
![[Pasted image 20231208001421.png]]
una vez tengamos el tipo de hash podemos decodearlo y asi encontar la contraseña en resumen tenemos un monton de usuarios y un monton de contraseñas que podemos usar para hacer ataques de fuerza bruta
### Fuerza bruta con hydra al puerto 110 
![[Pasted image 20231208001035.png]]
![[Pasted image 20231208001115.png]]
### entramos al puerto 110/pop3 y tenemos dos archivos debemos mirarlos
![[Pasted image 20231208230437.png]]
enumeramos y gardamos todos los nuevos usuarios y las contraseñas a nuestros diccionarios
### hydra por ssh
![[Pasted image 20231208231209.png]]
### ssh
![[Pasted image 20231208231402.png]]
### Escalada de privilegios
![[Pasted image 20231208235833.png]]
### encontramos un archivo que parece ser el estilo que se ejecuta cuando iniciamos session por ssh. el grupo de users puede escribir sobre el y el usuario actual pertenece a ese grupo asi que podemos editar el archivo y poner