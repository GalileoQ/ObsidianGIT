# Obtener el nombre de usuario
Usaremos el comando hostname:
![[Pasted image 20240612161320.png]]

# OBTENER INFORMACIÓN DEL SISTEMA
Para obtener información del sistema, usamos el comando systeminfo:
![[Pasted image 20240612161325.png]]
# OBTENER INFORMACIÓN SOBRE ACTUALIZACIONES DEL SISTEMA
Podemos también obtener información sobre las actualizaciones del sistema con el siguiente comando:
```powershell
wmic qfe get Caption,Description,HotFixID,InstalledOn
```

![[Pasted image 20240612161329.png]]