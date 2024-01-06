
### nmap
```python
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.0 (Ubuntu)
| http-git: 
|   10.10.31.85:80/.git/
|     Git repository found!
|_    Repository description: Unnamed repository; edit this file 'description' to name the...
|_http-server-header: nginx/1.14.0 (Ubuntu)
|_http-title: Super Awesome Site!
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 80.32 seconds
```
### Enumeracion del puerto 80
parece que contiene un repositorio en github
![[Pasted image 20240106114818.png]]
### Descargamos la herramienta gitdumper y 