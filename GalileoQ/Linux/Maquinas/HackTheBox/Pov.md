### Ping
```python
ping -c 1 10.10.11.251
PING 10.10.11.251 (10.10.11.251) 56(84) bytes of data.
64 bytes from 10.10.11.251: icmp_seq=1 ttl=127 time=157 ms

--- 10.10.11.251 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 156.729/156.729/156.729/0.000 ms
```

### TTL 127= Windows
### nmap
```python
PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 10.0
|_http-title: pov.htb
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.52 seconds
```
obtenemos el nombre de dominio del host y lo agregamos al /etc/hosts

### Enumeración del puerto http (80)

![[Pasted image 20240310205202.png]]

### Fuzzing con wfuzz (Directorios)
no obtenemos nada interesante por el momento
![[Pasted image 20240310205503.png]]

### Fuzzing con wfuzz (Subdominios)
encontramos un subdominio sospechoso. lo agregamos al /etc/hosts
![[Pasted image 20240310210838.png]]

### Web dev.pov.htb

![[Pasted image 20240311105847.png]]

### BurpSuite
interceptamos la petición de download cv
![[Pasted image 20240310212727.png]]
podemos interceptar esta petición y llamar al /web.config. esto  

### Vulnerabilidad Exploiting __VIEWSTATE
![[Pasted image 20240311105353.png]]


![[Pasted image 20240311105747.png]]