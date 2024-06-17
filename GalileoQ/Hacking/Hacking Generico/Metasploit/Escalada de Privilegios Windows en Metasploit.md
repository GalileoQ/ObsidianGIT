El primer paso será conseguir una sesión de meterpreter dentro de una máquina windows, por ejemplo en mi caso desde una máquina windows 7:
![[Pasted image 20240613010812.png]]

Pasamos la sesión al segundo plano y utilizamos el siguiente exploit:
![[Pasted image 20240613010816.png]]

Y este exploit se encargará de recolectar distintos exploits para escalar privilegios:
![[Pasted image 20240613010829.png]]

Y nos encuentra exploits válidos y otros que no:
![[Pasted image 20240613010843.png]]

Por ejemplo en nuestro caso vamos a utilizar el siguiente exploit (el último de los que aparecen en color verde):
```
use exploit/windows/local/tokenmagic
```

![[Pasted image 20240613010856.png]]
