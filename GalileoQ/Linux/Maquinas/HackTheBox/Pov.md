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

