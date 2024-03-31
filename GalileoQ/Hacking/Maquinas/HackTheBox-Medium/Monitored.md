#medium #Linux 
### Ping
```python
ping -c 1 10.10.11.248
PING 10.10.11.248 (10.10.11.248) 56(84) bytes of data.
64 bytes from 10.10.11.248: icmp_seq=1 ttl=63 time=150 ms

--- 10.10.11.248 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 150.024/150.024/150.024/0.000 ms
```

### TTL 63=Linux

### nmap
```python
# Nmap 7.94SVN scan initiated Sat Mar 30 20:10:43 2024 as: nmap -p- --open -sC -sV --min-rate 5000 -Pn -oN Scan 10.10.11.248
Nmap scan report for 10.10.11.248
Host is up (0.16s latency).
Not shown: 57867 closed tcp ports (reset), 7664 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 61:e2:e7:b4:1b:5d:46:dc:3b:2f:91:38:e6:6d:c5:ff (RSA)
|   256 29:73:c5:a5:8d:aa:3f:60:a9:4a:a3:e5:9f:67:5c:93 (ECDSA)
|_  256 6d:7a:f9:eb:8e:45:c2:02:6a:d5:8d:4d:b3:a3:37:6f (ED25519)
80/tcp   open  http       Apache httpd 2.4.56
|_http-server-header: Apache/2.4.56 (Debian)
|_http-title: Did not follow redirect to https://nagios.monitored.htb/
443/tcp  open  ssl/http   Apache httpd 2.4.56 ((Debian))
|_http-server-header: Apache/2.4.56 (Debian)
|_http-title: Nagios XI
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=nagios.monitored.htb/organizationName=Monitored/stateOrProvinceName=Dorset/countryName=UK
| Not valid before: 2023-11-11T21:46:55
|_Not valid after:  2297-08-25T21:46:55
5667/tcp open  tcpwrapped
Service Info: Host: nagios.monitored.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Mar 30 20:11:23 2024 -- 1 IP address (1 host up) scanned in 40.13 seconds
```

