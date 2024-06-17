Windows PrivescCheck escanea el sistema en busca de diferentes configuraciones, permisos, vulnerabilidades o servicios inseguros que podrían ser explotados para lograr una escalada de privilegios.

```bash
https://github.com/itm4n/PrivescCheck
```

-------------------------

Somos el usuario student:
![[Pasted image 20240612161402.png]]

Y lo ejecutamos con este parámetro:
```bash
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck"
```
Y en este caso nos encuentra unas credenciales:
![[Pasted image 20240612161406.png]]

Ahora para autenticarnos como el usuario administrador, ejecutaremos el comando runas.exe /user:administrator cmd:
![[Pasted image 20240612161413.png]]

