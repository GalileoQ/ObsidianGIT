#windows #Easy 
### nmap
```python
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=WIN-LU09299160F
| Not valid before: 2023-12-04T00:54:30
|_Not valid after:  2024-06-04T00:54:30
|_ssl-date: 2023-12-05T01:06:32+00:00; 0s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: WIN-LU09299160F
|   NetBIOS_Domain_Name: WIN-LU09299160F
|   NetBIOS_Computer_Name: WIN-LU09299160F
|   DNS_Domain_Name: WIN-LU09299160F
|   DNS_Computer_Name: WIN-LU09299160F
|   Product_Version: 10.0.17763
|_  System_Time: 2023-12-05T01:05:31+00:00
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 211.77 seconds
```

### Ports: 80/http - 3389/ms-wbt-server

### Enumeracion del puerto 80
![[Pasted image 20240612135521.png]]
parece que no hay nada importante

### Fuzzing con gobuster
![[Pasted image 20240612135527.png]]
tenemos un monton de directorios asi que vamos a buscar en todos

### buscamos en el primer directorio llamado search e investigamos el codigo fuente 
![[Pasted image 20240612135536.png]]

### authors
![[Pasted image 20240612135540.png]]

### tenemos un archivo robots.txt
![[Pasted image 20240612135546.png]]
posible contraseña

### tenemos credenciales para administrador vamos a conectarnos por RemoteDesktop
![[Pasted image 20240612135552.png]]
activamos los archivos ocultos y tenemos un backup dentro tenemos un archivo txt cambiamos los permisos y conseguimos la clave de admin para poder acceder al directorio de administrador

### WE ARE ROOT
