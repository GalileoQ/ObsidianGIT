Primero necesitamos obtener intrusión a la máquina víctima; y una vez dentro, pasamos la sesión a segundo plano y usamos el módulo de hashdump:
```bash
use post/windows/gather/hashdump
```
![[Pasted image 20240110153004.png]]
![[Pasted image 20240110153023.png]]
Y obtenemos los hashes de los usuarios:
![[Pasted image 20240110153039.png]]
