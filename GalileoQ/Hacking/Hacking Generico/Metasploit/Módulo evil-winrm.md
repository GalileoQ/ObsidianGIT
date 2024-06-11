Podemos acceder mediante evil-winrm usando metasploit de la siguiente forma:
```bash
use auxiliary/scanner/winrm/winrm_cmd
```
![[Pasted image 20230724124351.png]]
Ponemos las credenciales:
![[Pasted image 20230724124458.png]]
## FUERZA BRUTA WINRM
Podemos realizar ataques de fuerza bruta al servicio de winrm de windows con el siguiente módulo donde se nos piden las siguientes configuraciones:
```bash
use /auxiliary/scanner/winrm/winrm_login
```
![[Pasted image 20230724124649.png]]
## INICIAR SESIÓN WINRM
Una vez obtenidas credenciales, podemos iniciar sesión por winrm utilizando el siguiente módulo donde se nos pide la siguiente configuración:
```bash
exploit/windows/winrm/winrm_script_exec
```
![[Pasted image 20230724125023.png]]
