Para practicar el port forwarding con ssh vamos a practicar sobre la máquina [[MAQUINA INTERNAL (wordpress wpscan, hydra http, portforwarding chisel y hacking jenkins en docker)]], donde tenemos las siguientes credenciales para acceder vía SSH:
```
aubreanna:bubb13guM!@#123
```
![[Pasted image 20231001125448.png]]
Y vemos que esta máquina tiene un jenkins corriendo por su puerto 8080:
![[Pasted image 20230819120025.png]]
Vemos que efectivamente existe esta IP:
![[Pasted image 20230819125930.png]]
También podemos confirmar si un puerto interno tiene un servicio corriendo con el comando netstat -tuln:
```
netstat -tuln
```
![[Pasted image 20231001130051.png]]
Por tanto, desde nuestra máquina atacante ponemos el siguiente comando para decir que en mi puerto 5000 de mi máquina atacante esté corriendo el puerto interno 8080 de la máquina víctima:
```bash
ssh -L 5000:localhost:8080 aubreanna@10.10.218.157
```
![[Pasted image 20231001125819.png]]
Y si ahora accedemos a mi puerto 5000 de mi máquina local vemos que accedemos correctamente:
![[Pasted image 20231001125841.png]]
