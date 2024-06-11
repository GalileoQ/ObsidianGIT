El primer paso será levantar un servidor web, donde debemos descomprimir el fichero zip:
![[Pasted image 20231018080855.png]]
Teniendo listo los archivos para lanzar el servidor web:
![[Pasted image 20231018080939.png]]
E instalamos nodejs y npm, que será lo que nos haga falta para levantar el servidor web:
![[Pasted image 20231018081033.png]]
Y ejecutamos estos dos comandos:
![[Pasted image 20231018081300.png]]
Analizando el archivo index.js vemos que lo que hará será levantar un servidor web por el puerto 8080:
![[Pasted image 20231018081355.png]]
Levantamos el servidor web:
![[Pasted image 20231018081513.png]]
Y si accedemos a nuestro puerto 8080 vemos el servidor levantado:
![[Pasted image 20231018081535.png]]
Ahora vamos a publicar en tor este servidor web:
![[Pasted image 20231018081653.png]]
Y editaremos este archivo:
![[Pasted image 20231018081722.png]]
Una vez dentro, localizamos esta ruta:
![[Pasted image 20231018081830.png]]
Y tenemos que modificarlo para contemplar el hecho de que el servidor se ejecute por el puerto 8080 y quitando los comentarios:
```
#HiddenServiceDir /var/lib/tor/hidden_service/
#HiddenServicePort 80 127.0.0.1:80

Quedando

HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:8080
```
![[Pasted image 20231018082026.png]]
Reiniciamos el servicio tor y lo volvemos a levantar:
![[Pasted image 20231018082109.png]]
Y al ejecutar tor nos encontramos con el siguiente error, donde nos pide ejecutar tor como otro usuario:
![[Pasted image 20231018082610.png]]
Ya que vemos que al instalar tor se ha creado un usuario llamado debian-tor:
![[Pasted image 20231018082706.png]]
Por tanto, ejecutamos el siguiente comando y reiniciamos el servicio de tor; y así ya habremos asignado correctamente el permiso al usuario root:
```
sudo chown -R root:root /var/lib/tor/hidden_service/
```
![[Pasted image 20231018083018.png]]
Ejecutamos el comando tor y ya lo tenemos:
![[Pasted image 20231018083233.png]]
Ejecutamos el comando tor y lo vemos levantado:
![[Pasted image 20231018083322.png]]
Una vez hecho todo lo anterior, leemos la dirección .onion del fichero hostname:
![[Pasted image 20231018083652.png]]
Y desde un navegador tor lo abrimos, por lo que nos descargamos tor, lo descomprimimos y lo ejecutamos:
```bash
tar -xvf tor-browser-linux-x86_64-13.0.tar.xz
```
![[Pasted image 20231018084049.png]]
![[Pasted image 20231018084506.png]]
Y si ahora pegamos el .onion en tor ya habrá cargado la página:
![[Pasted image 20231018084617.png]]


