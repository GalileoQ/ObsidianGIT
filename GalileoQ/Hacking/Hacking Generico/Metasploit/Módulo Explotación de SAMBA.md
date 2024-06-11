Supongamos que nos encontramos con un servicio de SAMBA en una versión vulnerable:
![[Pasted image 20230802123053.png]]
Podemos buscar en metasploit algún módulo para enumerar y explotar este servicio:
```bash
use auxiliary/scanner/smb/smb_version
```
Usamos el módulo, ponemos la IP objetivo y comprobamos la versión exacta:
![[Pasted image 20230802123307.png]]
Lo buscamos con searchsploit y vemos que hay un exploit para esta versión:
![[Pasted image 20230802123357.png]]
Lo ponemos en metasploit, lo ejecutamos y estamos dentro:
```bash
use exploit/multi/samba/usermap_script
```
![[Pasted image 20230802123452.png]]
