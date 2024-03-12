### Ping 
```python
ping -c 1 10.10.11.233
PING 10.10.11.233 (10.10.11.233) 56(84) bytes of data.
64 bytes from 10.10.11.233: icmp_seq=1 ttl=63 time=152 ms

--- 10.10.11.233 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 152.321/152.321/152.321/0.000 ms
```

### TTL 63=Linux

### nmap
```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://analytical.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Mar 12 12:35:20 2024 -- 1 IP address (1 host up) scanned in 29.46 seconds
```

### Ports

| PORT   | STATE | SERVICE | VERSION               |
| ------ | ----- | ------- | --------------------- |
| 22/tcp | open  | ssh     | OpenSSH 8.9p1         |
| 80/tcp | open  | http    | nginx 1.18.0 (Ubuntu) |

### /etc/hosts
agregamos la IP y el dominio al /etc/hosts
![[Pasted image 20240312124121.png]]

### Enumeración del puerto 80 (http)
tenemos un apartado de login
![[Pasted image 20240312124841.png]]

agregamos el nuevo dominio al /etc/host 
![[Pasted image 20240312124810.png]]
conseguimos una vulnerabilidad para para metabase la cual nos permite autenticarnos [[https://github.com/Pyr0sec/CVE-2023-38646]]

### BurpSuite
interceptamos la petición del login y redireccionamos el GET

```python
/api/session/properties
```

![[Pasted image 20240312134206.png]]

### Ejecutamos el exploit 

![[Pasted image 20240312134618.png]]