# CONOCER QUIÉN TIENE ACCESO AL SISTEMA Y INFORMACIÓN DEL USUARIO
Comando getuid y net users:
![[Pasted image 20240612161230.png]]

O también con el comando net users:
![[Pasted image 20240612161234.png]]

Y conocer más información sobre el usuario:
![[Pasted image 20240612161246.png]]
# CONOCER PRIVILEGIOS DEL SISTEMA
Comando getprivs:
```bash
getprivs
```

![[Pasted image 20240612161252.png]]
O también privilegios de mi actual usuario con el siguiente comando:
```
whoami /priv
```

![[Pasted image 20240612161300.png]]
# ENUMERAR GRUPOS
Podemos ver los grupos en el sistema:
```bash
net localgroup
```

![[Pasted image 20240612161308.png]]
Y también podemos sacar informacíon del grupo al que pertenece un determinado usuario:
```bash
net localgroup administrators
```

![[Pasted image 20240612161313.png]]