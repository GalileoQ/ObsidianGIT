Vamos a utilizar la máquina windows 7 que ya tiene habilitado el login por smb:
![[Pasted image 20240613010418.png]]

Vemos que desde Kali podemos iniciar sesión:
![[Pasted image 20240613010422.png]]

Por tanto, podemos intentar iniciar sesión por smb usando hydra, que lo haríamos de la siguiente forma:
```bash
hydra -l mario -P /usr/share/wordlists/rockyou.txt smb://192.168.0.28
```

![[Pasted image 20240613010427.png]]

Aunque también podemos hacer otro ataque de fuerza bruta con el módulo de use auxiliary/scanner/smb/smb_login:
![[Pasted image 20240613010435.png]]

Realizamos las configuraciones añadiendo el diccionario (rockyou o unix_passwords, RHOSTS y lanzamos el ataque):
![[Pasted image 20240613010440.png]]

Y nos encuentra la contraseña:
![[Pasted image 20240613010446.png]]

Una vez hecho esto, es posible que podamos entrar dentro de la máquina por psexec usando el siguiente módulo:
```bash
use exploit/windows/smb/psexec
```
Donde le cargamos los datos para iniciar sesión obtenidos anteriormente:
![[Pasted image 20240613010459.png]]

Pero vemos que en este caso no funcionó:
![[Pasted image 20240613010505.png]]

Pero sí que podemos entrar por smb y enumerar usando herramientas como smbmap:
```bash
smbmap -H 192.168.0.28 -u mario -p 123123 -r Users
```

![[Pasted image 20240613010522.png]]

Y una vez hayamos enumerado el recurso compartido ya podremos acceder a él usando smbclient:
```bash
smbclient -U 'mario' //192.168.0.28/Users
```

![[Pasted image 20240613010528.png]]
