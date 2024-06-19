#ActiveDirectory 

Esta herramienta sirve para dumpear los hashes de todos los usuarios del dominio utilizando las credenciales de un usuario y contraseña que tengan el permiso para ello:
![[Pasted image 20240615211749.png]]

[[MAQUINA ATTACKTIVE DIRECTORY (enum4linux, kerbrute enumerar usuarios, ASREPRoasting obtener TGT, john the ripper, smbclient para listar recursos compartidos, secredump dumpear hashes y pass the hash)]]

---------------------------

# OTRO EJEMPLO

En caso de tener unas credenciales válidas, podemos usar secretsdump de impacket para listas hashes de usuarios del sistema, y vemos que podemos listar varios de ellos, entre los que se encuentra el del administrador:
![[Pasted image 20240615211756.png]]

Y esta es la contraseña:
```php
c2597747aa5e43022a3a3049a3c3b09d
```

![[Pasted image 20240615211803.png]]