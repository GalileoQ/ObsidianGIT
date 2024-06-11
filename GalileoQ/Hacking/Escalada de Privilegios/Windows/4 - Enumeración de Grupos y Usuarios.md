# CONOCER QUIÉN TIENE ACCESO AL SISTEMA Y INFORMACIÓN DEL USUARIO
Comando getuid y net users:
![[Pasted image 20230731133508.png]]
O también con el comando net users:
![[Pasted image 20230731133656.png]]
Y conocer más información sobre el usuario:
![[Pasted image 20230731133732.png]]
# CONOCER PRIVILEGIOS DEL SISTEMA
Comando getprivs:
```bash
getprivs
```
![[Pasted image 20230731133538.png]]
O también privilegios de mi actual usuario con el siguiente comando:
```
whoami /priv
```
![[Pasted image 20230731133620.png]]
# ENUMERAR GRUPOS
Podemos ver los grupos en el sistema:
```bash
net localgroup
```
![[Pasted image 20230731133840.png]]
Y también podemos sacar informacíon del grupo al que pertenece un determinado usuario:
```bash
net localgroup administrators
```
![[Pasted image 20230731133918.png]]
