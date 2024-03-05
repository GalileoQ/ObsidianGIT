### Ping 
```python
ping -c 1 10.10.11.249
PING 10.10.11.249 (10.10.11.249) 56(84) bytes of data.
64 bytes from 10.10.11.249: icmp_seq=1 ttl=127 time=235 ms

--- 10.10.11.249 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 235.207/235.207/235.207/0.000 ms
```

### TTL 127= Wondows
### nmap
```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-02-10 22:17 EST
Stats: 0:00:39 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 73.53% done; ETC: 22:18 (0:00:14 remaining)
Nmap scan report for 10.10.11.249
Host is up (0.097s latency).
Not shown: 65533 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE   VERSION
80/tcp    open  http      Microsoft IIS httpd 10.0
|_http-title: Did not follow redirect to http://crafty.htb
|_http-server-header: Microsoft-IIS/10.0
10.10.11.249:25565/tcp open  minecraft Minecraft 1.16.5 (Protocol: 127, Message: Crafty Server, Users: 3/100)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

### Ports: 80/http - 25565/tcp

### Enumeración del puerto 80

agregamos el subdominio al que nos esta redirigiendo en el /etc/host
![[Pasted image 20240210232547.png]]

pagina web/ tenemos lo que parece ser un subdominio diferente llamdo play.crafty.htb
![[Pasted image 20240210233121.png]]

### Fuzzing con gobuster

![[Pasted image 20240210234241.png]]

### Tlauncher
debido a que tenemos un servidor en python
![[Pasted image 20240305171536.png]]