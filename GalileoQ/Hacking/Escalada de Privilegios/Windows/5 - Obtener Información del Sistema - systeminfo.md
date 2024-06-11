# Obtener el nombre de usuario
Usaremos el comando hostname:
![[Pasted image 20230731132954.png]]
# OBTENER INFORMACIÓN DEL SISTEMA
Para obtener información del sistema, usamos el comando systeminfo:
![[Pasted image 20230731133022.png]]
# OBTENER INFORMACIÓN SOBRE ACTUALIZACIONES DEL SISTEMA
Podemos también obtener información sobre las actualizaciones del sistema con el siguiente comando:
```powershell
wmic qfe get Caption,Description,HotFixID,InstalledOn
```
![[Pasted image 20230731133118.png]]
