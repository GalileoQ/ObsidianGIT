### nmap
```python
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 90:35:66:f4:c6:d2:95:12:1b:e8:cd:de:aa:4e:03:23 (RSA)
|   256 53:9d:23:67:34:cf:0a:d5:5a:9a:11:74:bd:fd:de:71 (ECDSA)
|_  256 a2:8f:db:ae:9e:3d:c9:e6:a9:ca:03:b1:d7:1b:66:83 (ED25519)
80/tcp  open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Fowsniff Corp - Delivering Solutions
|_http-server-header: Apache/2.4.18 (Ubuntu)
110/tcp open  pop3    Dovecot pop3d
|_pop3-capabilities: TOP RESP-CODES UIDL SASL(PLAIN) PIPELINING AUTH-RESP-CODE CAPA USER
143/tcp open  imap    Dovecot imapd
|_imap-capabilities: Pre-login AUTH=PLAINA0001 more have IDLE post-login listed OK capabilities SASL-IR IMAP4rev1 LITERAL+ LOGIN-REFERRALS ID ENABLE
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.59 seconds
```
### Ports: 22/ssh - 80/hhtp - 110/pop3 143/imap

### Enumeracion del puerto 80
![[Pasted image 20231207231157.png]]
