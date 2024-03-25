Ahora dentro de la máquina friendly 1 tengo que pasarme el chisel, primero compartiéndolo a través de un servidor http con python pasándo por el proxychains:
![[Pasted image 20230816161253.png]]
![[Pasted image 20230816161218.png]]
Y ahora lo que debemos hacer es primero ejecutar el socat dentro de friendly 3, de tal forma que diremos que el tráfico del puerto 4444 lo envíe al puerto 1234 de nuestra máquina atacante, que es donde tendremos el chisel:
```bash
./socat tcp-l:4444,fork,reuseaddr tcp:192.168.0.30:1234
```
![[Pasted image 20230816161901.png]]
Y ahora con chisel, desde la máquina friendly 1, tenemos que decir que todo el tráfico lo envíe a la máquina friendly 3 donde tenemos el socat, que en este caso la friendly 3 tiene la IP 192.168.0.21:
```bash
./chisel client 192.168.0.21:4444 R:8888:socks
```
![[Pasted image 20230822122607.png]]
Y habremos recibido la conexión en nuestra máquina local con el chisel:
![[Pasted image 20230816162216.png]]
Ahora vamos a editar el archivo de proxychains y descomentamos la línea donde dice scritc_chain:
![[Pasted image 20230816162618.png]]
Y comentamos esta línea:
![[Pasted image 20230816162719.png]]
Y abajo del todo añadimos el nuevo túnel que habíamos creado por el puerto 8888:
![[Pasted image 20230816162645.png]]
Y ahora si hacemos un whatweb a esta IP vemos que llegamos correctamente:
![[Pasted image 20230822183346.png]]
Y la vemos en el navegador:
![[Pasted image 20230822183359.png]]
Y podemos incluso enviarnos una reverse shell y entablar conexión desde esta máquina a la atacante:
![[Pasted image 20230822183902.png]]
![[Pasted image 20230822183909.png]]
En esta máquina nos volvemos a bajar el chisel:
![[Pasted image 20230822184710.png]]
