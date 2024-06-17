El primer paso será levantar un servidor web, donde debemos descomprimir el fichero zip:
![[Pasted image 20240612233330.png]]

Teniendo listo los archivos para lanzar el servidor web:
![[Pasted image 20240612233333.png]]

E instalamos nodejs y npm, que será lo que nos haga falta para levantar el servidor web:
![[Pasted image 20240612233338.png]]

Y ejecutamos estos dos comandos:
![[Pasted image 20240612233345.png]]

Analizando el archivo index.js vemos que lo que hará será levantar un servidor web por el puerto 8080:
![[Pasted image 20240612233403.png]]

Levantamos el servidor web:
![[Pasted image 20240612233412.png]]

Y si accedemos a nuestro puerto 8080 vemos el servidor levantado:
![[Pasted image 20240612233419.png]]

Ahora vamos a publicar en tor este servidor web:
![[Pasted image 20240612233426.png]]

Y editaremos este archivo:
![[Pasted image 20240612233429.png]]

Una vez dentro, localizamos esta ruta:
![[Pasted image 20240612233434.png]]

Y tenemos que modificarlo para contemplar el hecho de que el servidor se ejecute por el puerto 8080 y quitando los comentarios:
```
#HiddenServiceDir /var/lib/tor/hidden_service/
#HiddenServicePort 80 127.0.0.1:80

Quedando

HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:8080
```

![[Pasted image 20240612233443.png]]
Reiniciamos el servicio tor y lo volvemos a levantar:
![[Pasted image 20240612233448.png]]

Y al ejecutar tor nos encontramos con el siguiente error, donde nos pide ejecutar tor como otro usuario:
![[Pasted image 20240612233503.png]]

Ya que vemos que al instalar tor se ha creado un usuario llamado debian-tor:
![[Pasted image 20240612233509.png]]

Por tanto, ejecutamos el siguiente comando y reiniciamos el servicio de tor; y así ya habremos asignado correctamente el permiso al usuario root:
```
sudo chown -R root:root /var/lib/tor/hidden_service/
```

![[Pasted image 20240612233520.png]]

Ejecutamos el comando tor y ya lo tenemos:
![[Pasted image 20240612233526.png]]

Ejecutamos el comando tor y lo vemos levantado:
![[Pasted image 20240612233530.png]]

Una vez hecho todo lo anterior, leemos la dirección .onion del fichero hostname:
![[Pasted image 20240612233542.png]]

Y desde un navegador tor lo abrimos, por lo que nos descargamos tor, lo descomprimimos y lo ejecutamos:
```bash
tar -xvf tor-browser-linux-x86_64-13.0.tar.xz
```
![[Pasted image 20240612233548.png]]

![[Pasted image 20240612233552.png]]

Y si ahora pegamos el .onion en tor ya habrá cargado la página:

![[Pasted image 20240612233557.png]]

