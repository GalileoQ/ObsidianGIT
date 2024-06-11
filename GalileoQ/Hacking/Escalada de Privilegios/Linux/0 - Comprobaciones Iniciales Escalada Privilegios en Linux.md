**COMPROBACIÓN 1**
Hay una página web que nos muestra todo un listado de los binarios SUID explotables, la cual es la siguiente:
```bash
https://gtfobins.github.io/
```
![[Pasted image 20230522162049.png]]
Por ejemplo vamos a buscar por el binario /sur/bin/env, y vemos como nos muestra la forma de explotar dicho binario por SUID:
![[Pasted image 20230522162916.png]]


**COMPROBACIÓN 2**
Listar las tareas de crontab para ver si el usuario root va a ejecutar algo que nosotros podamos modificar. Para ello usamos el comando cat /etc/crontab:
![[Pasted image 20230128140306.png]]
#### COMPROBACION 3 - **COMPROBACIÓN 4**
Muchas veces dentro del directorio config se almacenan credenciales que podemos utilizar para escalar privilegios, como en este caso por ejemplo:
![[Pasted image 20230128140408.png]]
Muchas veces las contraseñas guardadas en este directorio son de bases de datos, como es en este caso. Así que podemos ejecutar el comando mysql -u (usuario) -p (contraseña):
![[Pasted image 20230128140416.png]]
Y por ejemplo en este caso hay una tabla donde se guarda unas credenciales:
![[Pasted image 20230128140427.png]]
**COMPROBACIÓN 5**
Comprobamos en qué grupos se encuentra el usuario que somos con el comando id:
![[Pasted image 20230128140441.png]]
**COMPROBACIÓN 6**
Vamos a comprobar las capabilities con el comando getcap -r / 2>/dev/null, donde por ejemplo en este caso podemos ejecutar código Python:
![[Pasted image 20230128140452.png]]
Si podemos ejecutar código Python podremos importar el módulo OS y ejecutar un comando para lanzarnos una bash como root:
![[Pasted image 20230401015651.png]]
**COMPROBACIÓN 7**
Es muy importante comprobar que no haya archivos ocultos por ahí. Para ello ejecutamos el comando ls -la, y como se peude ver en el ejemplo, encontramos un archivo oculto llamado backup:
![[Pasted image 20230128140514.png]]
![[Pasted image 20230128140518.png]]
**COMPROBACIÓN 8**
Ver si tenemos el poder de hacer un append a un script para añadir código que nos de una reverse shell o eleve nuestros privilegios, y en este caso sí tenemos el permiso de añadir algo, un append:
![[Pasted image 20230128140529.png]]
Por tanto hacemos un append de esta manera, donde esto se añadirá al final del script:
![[Pasted image 20230128140537.png]]