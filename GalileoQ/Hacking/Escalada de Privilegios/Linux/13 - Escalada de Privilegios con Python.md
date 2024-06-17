Si encontramos un script de Python donde tengamos permisos de escritura y sea ejecutado por el administrador, podemos modificarlo para poder lanzarnos una shell como root con este código:
```python
import os

os.system("chmod u+s /bin/bash")
```
Ahora si conseguimos que este script sea ejecutado por el administrador (por ejemplo si es un script que se ejecuta con crontab) si luego usamos el comando bash -p, nos convertimos en usuario root:
![[Pasted image 20240612160254.png]]

![[Pasted image 20240612160257.png]]

--------------------------------------------------------------

## OTRO EJEMPLO ESCALAR PRIVILEGIOS CON PYTHON

Si hacemos una búsqueda de los permisos SUID, vemos que Python se encuentra entre ellos:
![[Pasted image 20240612160306.png]]

Y también vemos estos permisos para Python:
![[Pasted image 20240612160311.png]]

Por tanto nos creamos un fichero de Python con un código que nos permita escalar privilegios:
![[Pasted image 20240612160315.png]]

Lo compartimos con la máquina víctima:
![[Pasted image 20240612160319.png]]

Y lo ejecutamos para convertirnos en root:
![[Pasted image 20240612160405.png]]
![[Pasted image 20240612160409.png]]

[[MAQUINA ROOTME (Fuzzing, php malicioso camuflándolo como phtml en burpsuite, escalada de privilegios comprobando permisos SUID de Python)]]

## Python Library Hijacking

Podemos escalar privilegios inyectando código en las librerías de python si vemos que están siendo utilizadas en un script. Así como crear un script con código malicioso con el mismo nombre de la librería dentro del mismo directorio de trabajo, y así hacer que sea llamado por el script principal:
