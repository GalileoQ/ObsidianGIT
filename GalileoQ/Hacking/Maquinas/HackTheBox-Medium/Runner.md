#fuzzing #Linux #medium #nmap #tecnicas 
### Ping
```python
ping -c 1 10.10.11.13
PING 10.10.11.13 (10.10.11.13) 56(84) bytes of data.
64 bytes from 10.10.11.13: icmp_seq=1 ttl=63 time=76.3 ms

--- 10.10.11.13 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 76.279/76.279/76.279/0.000 ms
```

### TTL 63 = Linux

### nmap
```python
sudo nmap -p- --open -sC -sV --min-rate 3000 -n -Pn 10.10.11.13 -oN Scan
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-30 10:36 EDT
RTTVAR has grown to over 2.3 seconds, decreasing to 2.0
RTTVAR has grown to over 2.3 seconds, decreasing to 2.0
Nmap scan report for 10.10.11.13
Host is up (0.072s latency).
Not shown: 53707 closed tcp ports (reset), 11825 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp   open  http        nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://runner.htb/
8000/tcp open  nagios-nsca Nagios NSCA
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.30 seconds
```

### Enumeración del puerto 80 (http)
por el momento no vemos nada interesante. vamos a enumerar el puerto 8000
![[Pasted image 20240430104302.png]]

### Enumeración del puerto 8000 (http)
parece no poder encontrar el dominio. podemos hacer fuzzing para investigar mas a fondo
![[Pasted image 20240430105130.png]]

### Fuzzing con ffuzz (Directorios)
no encontramos gran cosa en la enumeración de directorios así que vamos a enumerar subdominios 
![[Pasted image 20240430105327.png]]

### Fuzzing con wfuzz (subtominios)
tenemos un subdominio algo interesante. 
![[Pasted image 20240430112533.png]]




![[Pasted image 20240430112300.png]]