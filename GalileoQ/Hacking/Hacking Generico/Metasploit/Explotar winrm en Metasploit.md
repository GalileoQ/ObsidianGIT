El puerto donde corre winrm por lo general es el 5985:
![[Pasted image 20230730162902.png]]
Y usaremos el siguiente módulo de metasploit:
```bash
use auxiliary/scanner/winrm/winrm_login
```
Y haremos la configuración:
![[Pasted image 20230730163115.png]]
Y nos lo encuentra:
![[Pasted image 20230730163236.png]]
Una vez hecho esto, tenemos que comprobar que mediante winrm tengamos permisos para loguearnos, por lo que usamos el siguiente módulo:
```bash
use auxiliary/scanner/winrm/winrm_auth_methods
```
Lo lanzamos y vemos que está habilitado:
![[Pasted image 20230730163416.png]]
Por último, ya podemos ejecutar un comando de forma remota proporcionando las credenciales de antes:
![[Pasted image 20230730163544.png]]
Por último, si queremos ganar acceso remoto, tenemos que usar el siguiente módulo:
```bash
use exploit/windows/winrm/winrm_script_exec
```
![[Pasted image 20230730163820.png]]
Lo ejecutamos y ya estamos dentro:
