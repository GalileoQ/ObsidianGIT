Primero necesitamos obtener intrusión a la máquina víctima; y una vez dentro, pasamos la sesión a segundo plano y usamos el módulo de hashdump:
```bash
use post/windows/gather/hashdump
```
![[Pasted image 20240613011209.png]]

![[Pasted image 20240613011217.png]]
Y obtenemos los hashes de los usuarios:

![[Pasted image 20240613011223.png]]