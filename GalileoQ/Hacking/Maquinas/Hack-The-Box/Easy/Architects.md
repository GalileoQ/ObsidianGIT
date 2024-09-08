![[banner.png]]

### Machine Name: Architects
---

`​06 Month-09-2024`

Machine Author(s): [GalileoQ](https://app.hackthebox.com/profile/1598457) - [b0ySie7e](https://app.hackthebox.com/users/417609) 

## Description:


Difficulty:<font color="orange">Medium</font> 

### Flags:

User: `5bb745d22fc34ff77f601754fcf3cc3a`
Root: `a6937a661804a28ea7e5e5d94958a09a`


### Credentials

| Users                     | Password             |
| ------------------------- | -------------------- |
| root (host architects)    | TDSDFwkl88!d48sTa    |
| tommy (host architects)   | vkghTAEmph789129LBFF |
| administrator (wordpress) | 6E*V4mNctSvE&4KINC   |
| galileo (wordpress)       | g.thornecroft        |
| jessica (wordpress)       | CaS#$FGSf45T         |
| root (mysql db)           | Ilo&a5aT5RO8Yt94P    |
| tomydb (mysql db user)    | ueM2790ZWFCFZBjvslMy |
| (keppass none user)       | galileo.thornecroft  |

### Firewall Rules: `NONE`
---
# WriteUp
## Enumeration whith nmap

`sudo nmap -p- -open -sCV --min-rate 5000 -n -Pn 10.2.2.17 -oN Scan`
En nuestro escaneo de nmap logramos identificar dos puertos abiertos. puerto 22/ssh y puerto 80 http 
```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-07 21:47 EDT
Nmap scan report for 10.2.2.17
Host is up (0.00070s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 a2:d7:ac:38:98:81:2c:e7:a5:04:8b:a3:29:5e:e0:e9 (ECDSA)
|_  256 81:45:33:c0:3a:57:3c:64:62:d3:51:45:92:44:40:e3 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-title: Did not follow redirect to http://architects.htb/
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 08:00:27:F1:E1:5A (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.70 seconds
```

