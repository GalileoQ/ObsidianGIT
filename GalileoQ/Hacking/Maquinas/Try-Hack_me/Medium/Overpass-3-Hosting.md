#Linux #medium 
### nmap 
```python
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 de:5b:0e:b5:40:aa:43:4d:2a:83:31:14:20:77:9c:a1 (RSA)
|   256 f4:b5:a6:60:f4:d1:bf:e2:85:2e:2e:7e:5f:4c:ce:38 (ECDSA)
|_  256 29:e6:61:09:ed:8a:88:2b:55:74:f2:b7:33:ae:df:c8 (ED25519)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
|_http-title: Overpass Hosting
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.37 (centos)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.86 seconds
```
### Ports: 21/ftp - 22/ssh - 80/http

### Enumeracion del puerto 80/http

![[Pasted image 20240612154124.png]]
tenemos un archivo llamado backups

### tenemos un archivo pgp
vamos a agregar la clave privada al archivo 
![[Pasted image 20240612154128.png]]
### pgp decrypt

![[Pasted image 20240612154132.png]]
hemos guardado la salida en un nuevo archivo llamado CustomerDetails.xlsx
### miramos el archivo con una herramienta online para excel

![[Pasted image 20240612154138.png]]
tenemos 3 usuarios y 3 password

### Enumeracion del puerto 21/fftp

![[Pasted image 20240612154142.png]]
el usuario paradox y la contraseña estan habilitados para ftp y tenemos permisos de escritura en el servicio ftp asi que usaremos esto para subir una reverse shell

### msfvenom
creamos un payload
![[Pasted image 20240612154147.png]]
### msfconsole
buscaremos la options /multi/handler
![[Pasted image 20240612154151.png]]
###### set options
![[Pasted image 20240612154157.png]]
LHOST(Local-Ip) - LPORT(Listen Meterpreter Port)  - set payload(payload de php meterpreter)
### subimos linpeas
uploads /Ruta/De/TU/linpeas.sh
![[Pasted image 20240612154202.png]]

### vulnerabilidad *(rw,fsid=0,sync,no_root_squash,insecure)*
esta vulnerabilidad nos indica que podemos crear una montura de /home/james
![[Pasted image 20240612154206.png]]
una vez termine el linpeas hacemos Ctrl z para enviar la bash a segundo plano 
### Configuración del Port Forwarding 

![[Pasted image 20240612154211.png]]
aqui hacemos que nuestro puerto 2049 se transforme en el puerto 2049 de la maquina victivo

aqui vemos que  tenemos una sesión activa. 
![[Pasted image 20240612154215.png]]
### Configuración del Port Forwarding con chisel (Opcional)
![[Pasted image 20240612154219.png]]
###### verificamos que tenemos conecxion en el puerto
![[Pasted image 20240612154223.png]]
### Creamos la montura
creamos un directorio en este caso lo llamare james y creamos la montura dentro de la carpeta james
![[Pasted image 20240612154232.png]]
### ssh james
podemos ver la id_rsa del usuario james
![[Pasted image 20240612154236.png]]
###### hacemos un cat para copiar la id_rsa
![[Pasted image 20240612154310.png]]
### Conexion ssh con usuario james

![[Pasted image 20240612154317.png]]
### Escalada de privilegios
hacemos una copia del binario /bin/bash a la montura de james y le cambiamos los permisos y el propietario desde la maquina atacante
```python
cp /bin/bash ~/Desktop/thm/Medium-Machine/Overpass3-Hosting/james > bash.sh
```

![[Pasted image 20240612154322.png]]
### WE ARE ROOT


![[Pasted image 20240612154325.png]]