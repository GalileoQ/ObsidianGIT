Comprobamos los procesos del sistema con el siguiente comando:
```
ps -ead
```
Y vemos que el crontab se está ejecutando:
![[Pasted image 20230801104244.png]]
Pero perdemos la conexión al poco tiempo:
![[Pasted image 20230801105214.png]]
Por lo que si entramos otra vez y rápidamente creamos en crontab una tarea para que se cree un servidor http que aloje los archivos del directorio de la máquina víctima con python:
```bash
echo "* * * * * cd /home/student/ && python -m SimpleHTTPServer" > cron # Esto en caso de usar python 2.
```
![[Pasted image 20230801104952.png]]
Una vez fuera, si lanzamos un nmap vemos el puerto 8000 abierto:
![[Pasted image 20230801105339.png]]
Y podemos obtener una flag:
![[Pasted image 20230801105408.png]]
