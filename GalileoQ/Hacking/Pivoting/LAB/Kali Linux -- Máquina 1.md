Desde la máquina Kali nos conectamos por ssh a la máquina 1 ya que tenemos conectividad:
![[Pasted image 20240615204048.png]]

Y en cada máquina tenemos que ejecutar el comando dhclient para que pueda reconocer la segunda interfaz:
![[Pasted image 20240615204053.png]]

Y ya estamos en la máquina 1, donde tenemos visibilidad con la máquina 2 que tiene la IP 20.20.20.11:

![[Pasted image 20240615204056.png]]