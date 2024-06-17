Lo primero será estar dentro de una máquina windows con un meterpreter y siendo usuario administrador:
![[Pasted image 20240615201037.png]]

Una vez hecho esto, usaremos el siguiente módulo, poniendo la sesión en segundo plano y luego volviendo a cargarla:
```bash
use exploit/windows/local/persistence_service
```

![[Pasted image 20240615201045.png]]

Ahora, por otra parte, nos ponemos a la escucha con metasploit para recibir una nueva conexión en caso de reiniciar la máquina:
![[Pasted image 20240615201054.png]]

Apagamos la máquina víctima:
![[Pasted image 20240615201106.png]]

Pero en la otra ventana de metasploit habremos recibido la conexión una vez que la máquina víctima se haya reiniciado:

![[Pasted image 20240615201112.png]]