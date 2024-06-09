#medium #tecnicas #windows #fuzzing 
### Ping
```python

```

### TTL

### nmap
```python
# Nmap 7.94SVN scan initiated Sun Jun  9 12:19:54 2024 as: nmap -p- --open -sC -sV --min-rate 3000 -n -Pn -oN Scan 10.10.11.19
Nmap scan report for 10.10.11.19
Host is up (0.15s latency).
Not shown: 65477 closed tcp ports (reset), 56 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 3e:21:d5:dc:2e:61:eb:8f:a6:3b:24:2a:b7:1c:05:d3 (RSA)
|   256 39:11:42:3f:0c:25:00:08:d7:2f:1b:51:e0:43:9d:85 (ECDSA)
|_  256 b0:6f:a0:0a:9e:df:b1:7a:49:78:86:b2:35:40:ec:95 (ED25519)
80/tcp open  http    nginx 1.18.0
|_http-title: Did not follow redirect to http://app.blurry.htb/
|_http-server-header: nginx/1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jun  9 12:20:32 2024 -- 1 IP address (1 host up) scanned in 38.23 seconds
```

