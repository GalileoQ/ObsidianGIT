
# nmap
```python
sudo nmap -p- -sCV 172.31.37.55

Starting Nmap 7.94 ( https://nmap.org ) at 2025-03-09 18:53 IST
Nmap scan report for vanguardtrust.bank (172.31.37.55)
Host is up (0.00024s latency).
Not shown: 65530 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
25/tcp   open  smtp          OpenSMTPD
| smtp-commands: 11b55bf54bac Hello vanguardtrust.bank [172.31.37.101], pleased to meet you, 8BITMIME, ENHANCEDSTATUSCODES, SIZE 36700160, DSN, HELP
|_ 2.0.0 This is OpenSMTPD 2.0.0 To report bugs in the implementation, please contact bugs@openbsd.org 2.0.0 with full details 2.0.0 End of HELP info
80/tcp   open  http          Apache httpd 2.4.41
|_http-generator: WordPress 6.7.1
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-title: Vanguard Trust Bank
|_http-server-header: Apache/2.4.41 (Ubuntu)
2222/tcp open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 ac:b8:2e:46:71:c0:4a:61:14:c1:34:aa:c4:22:3b:19 (RSA)
|   256 b6:83:d5:84:10:88:91:05:04:73:79:73:3f:95:8d:59 (ECDSA)
|_  256 7a:2c:47:21:7b:90:83:53:be:8d:07:56:e3:9c:1c:3a (ED25519)
3389/tcp open  ms-wbt-server xrdp
8080/tcp open  http-proxy
|_http-title: Login Page
|_http-open-proxy: Proxy might be redirecting requests
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 
|     Content-Type: text/html;charset=UTF-8

```