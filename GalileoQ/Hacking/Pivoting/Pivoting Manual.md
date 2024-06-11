Esta es la estructura de red que vamos a intentar replicar:
![[Pasted image 20230722161139.png]]
Lo primero será ir a la parte de herramientas y Administrador de red: 
![[Pasted image 20230717133814.png]]
Y ahora vamos a la parte de redes NAT y creamos nuestra primera red con estos parámetros:
![[Pasted image 20230717134004.png]]
Por tanto, una vez creada esta red sobre la que haremos pivoting, veremos como configuraremos las 3 máquinas:
MÁQUINA KALI:
![[Pasted image 20230717134609.png]]
MÁQUINA UBUNTU INTERMEDIA:
![[Pasted image 20230717134629.png]]
MÁQUINA METASPLOITABLE FINAL:
![[Pasted image 20230717134647.png]]
De tal forma que la máquina kali tiene conectividad solo con la máquina ubuntu; y la máquina ubuntu tiene conectividad tanto con la Kali como con la metasploitable:
![[Pasted image 20230717134751.png]]
# PIVOTING
Debemos ejecutar el siguiente comando:
![[Pasted image 20230720122135.png]]
De tal forma, que desde nuestra máquina atacante, tendremos visibilidad a la dirección 10.10.10.0/24, por ejemplo vamos a probar en llegar a la máquina metasploitable final:
![[Pasted image 20230720122210.png]]
# SSHUTTLE + SOCAT
Ejecutamos el siguiente comando en la máquina ubuntu, ya sea desde la reverse shell o desde la propia ubuntu:
```bash
socat tcp-l:443,fork,reuseaddr tcp:192.168.0.30:443
```
![[Pasted image 20230722163603.png]]
Por tanto, una vez hecho esto ya podremos atacar a la máquina metasploitable, por ejemplo con un exploit que explote el puerto 21:
![[Pasted image 20230722163945.png]]
