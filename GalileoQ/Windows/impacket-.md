### impacket-GetNPUsers:
Descripción: Extrae tickets TGT (Ticket Granting Ticket) sin necesidad de autenticación de contraseña, utilizando el protocolo Kerberos.
```python
impacket-GetNPUsers DOMAIN/USER -dc-ip <domain_controller_ip>
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
### impacket-esentutl**:
    
    - Descripción: Permite interactuar con bases de datos ESENT (Extensible Storage Engine) en sistemas Windows.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-esentutl.py`
        
5. **impacket-getArch**:
    
    - Descripción: Obtiene la arquitectura del sistema remoto (32 o 64 bits).
    - Ejemplo:
        
        bashCopy code
        
        `impacket-getArch.py DOMAIN/USER:PASSWORD@TARGET_HOST`
        
6. **impacket-mssqlclient**:
    
    - Descripción: Un cliente interactivo de SQL Server que se puede utilizar para ejecutar consultas SQL y comandos en un sistema remoto que ejecuta SQL Server.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-mssqlclient.py DOMAIN/USER:PASSWORD@TARGET_HOST`
        
7. **impacket-mssqlinstance**:
    
    - Descripción: Enumera las instancias de SQL Server en un sistema remoto.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-mssqlinstance.py DOMAIN/USER:PASSWORD@TARGET_HOST`
        
8. **impacket-mssqlping**:
    
    - Descripción: Realiza un ping a un servidor SQL para verificar la disponibilidad.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-mssqlping.py DOMAIN/USER:PASSWORD@TARGET_HOST`
        
9. **impacket-mssqlservice**:
    
    - Descripción: Enumera los servicios de SQL Server en un sistema remoto.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-mssqlservice.py DOMAIN/USER:PASSWORD@TARGET_HOST`
        
10. **impacket-ntlmrelayx**:
    
    - Descripción: Se utiliza para realizar ataques de relé NTLM en redes corporativas.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-ntlmrelayx.py -tf targets.txt`
        
11. **impacket-psexec**:
    
    - Descripción: Ejecuta comandos en un sistema remoto utilizando el servicio PsExec.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-psexec.py DOMAIN/USER:PASSWORD@TARGET_HOST CMD.EXE`
        
12. **impacket-rpcdump**:
    
    - Descripción: Realiza una enumeración de RPC en un host remoto.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-rpcdump.py DOMAIN/USER:PASSWORD@TARGET_HOST`
        
13. **impacket-secretsdump**:
    
    - Descripción: Extrae contraseñas almacenadas en la SAM (Security Accounts Manager) de un sistema Windows.
    - Ejemplo:
        
        bashCopy code
        
        `impacket-secretsdump.py DOMAIN/USER:PASSWORD@TARGET_HOST`