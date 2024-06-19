#
Esta es la estructura de red que vamos a intentar replicar:
![[Pasted image 20240615202743.png]]

Lo primero será ir a la parte de herramientas y Administrador de red: 
![[Pasted image 20240615202747.png]]

Y ahora vamos a la parte de redes NAT y creamos nuestra primera red con estos parámetros:
![[Pasted image 20240615202750.png]]

Por tanto, una vez creada esta red sobre la que haremos pivoting, veremos como configuraremos las 3 máquinas:
MÁQUINA KALI:
![[Pasted image 20240615202801.png]]

MÁQUINA UBUNTU INTERMEDIA:
![[Pasted image 20240615202819.png]]

MÁQUINA METASPLOITABLE FINAL:
![[Pasted image 20230717134647.png]]
De tal forma que la máquina kali tiene conectividad solo con la máquina ubuntu; y la máquina ubuntu tiene conectividad tanto con la Kali como con la metasploitable:
![[Pasted image 20240615202825.png]]
### PIVOTING
Debemos ejecutar el siguiente comando:
![[Pasted image 20240615202838.png]]

De tal forma, que desde nuestra máquina atacante, tendremos visibilidad a la dirección 10.10.10.0/24, por ejemplo vamos a probar en llegar a la máquina metasploitable final:
![[Pasted image 20240615202842.png]]
### SSHUTTLE + SOCAT
Ejecutamos el siguiente comando en la máquina ubuntu, ya sea desde la reverse shell o desde la propia ubuntu:
```python
socat tcp-l:443,fork,reuseaddr tcp:192.168.0.30:443
```

![[Pasted image 20240615202917.png]]

Por tanto, una vez hecho esto ya podremos atacar a la máquina metasploitable, por ejemplo con un exploit que explote el puerto 21:

![[Pasted image 20240615202920.png]]