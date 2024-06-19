#pivoting 
Vamos a tener el siguiente mapa de red con estas máquinas:
![[Pasted image 20240615202948.png]]

Desplegamos los 3 laboratorios:
![[Pasted image 20240615202950.png]]

Primero entramos en la máquina windows 7 explotando un eternalblue, y una vez dentro detectamos la segunda interfaz de red:
![[Pasted image 20240615202955.png]]

Ahora buscamos equipos dentro de esta red interna:
![[Pasted image 20240615203003.png]]

Después enrutamos el tráfico:
![[Pasted image 20240615203006.png]]

Y detectamos puertos abiertos dentro de la máquina objetivo:
![[Pasted image 20240615203014.png]]

Y ahora con portproxy hacemos el port forwarding para enviarnos el puerto 80 de la máquina víctima a nuestro puerto 8080:
![[Pasted image 20240615203018.png]]

Y ahora accedemos sin problema al puerto 80 de la máquina remota final:
![[Pasted image 20240615203023.png]]

Pero en esta máquina corría otro servicio web por su puerto 631, por lo que podemos traérnoslo también a nuestro puerto 631:
![[Pasted image 20240615203028.png]]

Y accedemos:
![[Pasted image 20240615203032.png]]

Encontramos también el usuario dimitri, por lo que podemos traernos también el puerto 22 para hacerle el ataque de fuerza bruta:
![[Pasted image 20240615203037.png]]

Nos lo traemos al puerto 222:
![[Pasted image 20240615203042.png]]

Y atacamos a este puerto con hydra poniendo la IP intermedia:
```bash
hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://192.168.0.28 -s 222
```
Y funciona:
![[Pasted image 20240615203047.png]]

![[Pasted image 20240615203051.png]]
