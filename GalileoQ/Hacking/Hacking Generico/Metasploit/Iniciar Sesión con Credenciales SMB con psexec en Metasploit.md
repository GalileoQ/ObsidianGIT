Utilizamos un ataque de fuerza bruta contra el protocolo smb y obtenemos las credenciales:
```bash
use auxiliary/scanner/smb/smb_login
```
![[Pasted image 20230730161556.png]]
Podemos usar el módulo de psexec para iniciar sesión con dichas credenciales:
```bash
use exploit/windows/smb/psexec
```
![[Pasted image 20230730161744.png]]

