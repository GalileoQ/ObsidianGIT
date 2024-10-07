#
### Ping

```python
ping -c 1 10.10.11.36
PING 10.10.11.36 (10.10.11.36) 56(84) bytes of data.
64 bytes from 10.10.11.36: icmp_seq=1 ttl=63 time=165 ms

--- 10.10.11.36 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 164.933/164.933/164.933/0.000 ms
```

### TTL = 63 Linux

### nmap

```python
sudo nmap -p- -open -sCV --min-rate 5000 -n -Pn 10.10.11.36 -oN Scan
[sudo] password for gleoq: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-07 13:57 EDT
Nmap scan report for 10.10.11.36
Host is up (0.17s latency).
Not shown: 65526 closed tcp ports (reset), 7 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a2:ed:65:77:e9:c4:2f:13:49:19:b0:b8:09:eb:56:36 (ECDSA)
|_  256 bc:df:25:35:5c:97:24:f2:69:b4:ce:60:17:50:3c:f0 (ED25519)
80/tcp open  http    Caddy httpd
|_http-title: Did not follow redirect to http://yummy.htb/
|_http-server-header: Caddy
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.04 seconds
```

### Enumeración Puerto 80 (http)
tenemos una web de restaurante donde podemos logearnos
![[Pasted image 20241007140518.png]]

`BOOK A TABLE`
![[Pasted image 20241007140721.png]]

`Dashboard`
en este punto podemos descargar `save Icalendar` que es un `ICS` 
![[Pasted image 20241007141112.png]]

`burpsuite`
al intentar descargar el a
![[Pasted image 20241007144434.png]]





### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
