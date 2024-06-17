Comando getuid:
![[Pasted image 20240613010702.png]]

Comando getprivs - Enumerar privilegios
![[Pasted image 20240613010710.png]]

Si lo quisiéramos hacer de forma manual sería de esta forma:
![[Pasted image 20240613010717.png]]

## Enumerar usuarios que iniciaron sesión:
Pasamos el meterpreter en segundo plano y ejecutamos este módulo:
![[Pasted image 20240613010724.png]]

Le cargamos la sesión correspondiente:
![[Pasted image 20240613010731.png]]

Ejecutamos el módulo y veríamos a más usuarios, aunque en esta ocasión sólo tenemos 1:
![[Pasted image 20240613010738.png]]

Si quisiéramos hacer esto de forma manual, sería con el comando query user:
![[Pasted image 20240613010748.png]]
# VER CUENTAS CREADAS EN EL SISTEMA
Para ver cuentas de usuario creadas en el sistema windows usaremos el comando net users:
![[Pasted image 20240613010752.png]]

Si quisiéramos obtener información de un usuario en particular, usaríamos el comando net user Administrator:
![[Pasted image 20240613010759.png]]