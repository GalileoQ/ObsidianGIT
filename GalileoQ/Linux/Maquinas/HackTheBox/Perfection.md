### Ping
```python
ping -c 1 10.10.11.253
PING 10.10.11.253 (10.10.11.253) 56(84) bytes of data.
64 bytes from 10.10.11.253: icmp_seq=1 ttl=63 time=372 ms

--- 10.10.11.253 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 371.851/371.851/371.851/0.000 ms
```
### TTL 63= Linux
### nmpa
```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 80:e4:79:e8:59:28:df:95:2d:ad:57:4a:46:04:ea:70 (ECDSA)
|_  256 e9:ea:0c:1d:86:13:ed:95:a9:d0:0b:c8:22:e4:cf:e9 (ED25519)
80/tcp open  http    nginx
|_http-title: Weighted Grade Calculator
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 53.08 seconds
```

### Enumeración del puerto 80
