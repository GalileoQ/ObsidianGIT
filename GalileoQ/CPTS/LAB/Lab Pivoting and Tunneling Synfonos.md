en este laboratorios vamos a realizar pruebas de pivoting y Tunneling entre 3 maquinas synfonos (1,2,3) 

primero vamos a identificar nuestra interfaz de red para saber en que segmento estamos operando y luego vamos a realizar un escaneo de nuestra red para identificar posibles maquinas que estén activas

```python
	ifconfig

	sudo apr-scan -I eth0 --localnet

	netdiscover -i eth0 -r ip
```

![[Pasted image 20240617144232.png]]

### MAQUINA SYNFONOS 1 - 10.0.2.15

### nmap

```python
# Nmap 7.94SVN scan initiated Mon Jun 17 15:26:13 2024 as: nmap -p- --open -sCV -sS -Pn -n --min-rate 5000 -oN Scan 10.0.2.15
Nmap scan report for 10.0.2.15
Host is up (0.0013s latency).
Not shown: 65530 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 ab:5b:45:a7:05:47:a5:04:45:ca:6f:18:bd:18:03:c2 (RSA)
|   256 a0:5f:40:0a:0a:1f:68:35:3e:f4:54:07:61:9f:c6:4a (ECDSA)
|_  256 bc:31:f5:40:bc:08:58:4b:fb:66:17:ff:84:12:ac:1d (ED25519)
25/tcp  open  smtp        Postfix smtpd
|_smtp-commands: symfonos.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=symfonos
| Subject Alternative Name: DNS:symfonos
| Not valid before: 2019-06-29T00:29:42
|_Not valid after:  2029-06-26T00:29:42
80/tcp  open  http        Apache httpd 2.4.25 ((Debian))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.25 (Debian)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.5.16-Debian (workgroup: WORKGROUP)
MAC Address: 08:00:27:19:95:4B (Oracle VirtualBox virtual NIC)
Service Info: Hosts:  symfonos.localdomain, SYMFONOS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.16-Debian)
|   Computer name: symfonos
|   NetBIOS computer name: SYMFONOS\x00
|   Domain name: \x00
|   FQDN: symfonos
|_  System time: 2024-06-17T14:26:26-05:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
| smb2-time: 
|   date: 2024-06-17T19:26:27
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: SYMFONOS, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jun 17 15:26:27 2024 -- 1 IP address (1 host up) scanned in 14.31 seconds
```

### Enumeración del puerto 80 (http)
analizamos e
![[Pasted image 20240617153736.png]]