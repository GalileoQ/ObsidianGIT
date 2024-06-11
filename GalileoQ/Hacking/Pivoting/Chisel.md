Chisel es una herramienta de pivoting compatible tanto con máquinas Windows como Linux. Nos permite de forma muy cómoda prácticamente obtener las mismas funciones que SSH (en el aspecto de Port Forwarding).

Tenemos un repositorio oficial para descargar la herramienta:
```bash
https://github.com/jpillora/chisel/releases
```

![[Pasted image 20230804090601.png]]
![[Pasted image 20230804090727.png]]
Este será el laboratorio:
![[Pasted image 20230804090827.png]]

------------------

Una vez con todo desplegado, tenemos que ejecutar chisel tanto en la máquina atacante como en la máquina windows 7 intermedia:
![[Pasted image 20230804091121.png]]
Y la versión de windows la descomprimimos en nuestro Linux y la compartimos con la máquina intermedia:
![[Pasted image 20230804091428.png]]
![[Pasted image 20230804091512.png]]
Y lo tenemos localizable:
![[Pasted image 20230804091549.png]]
Y si lo ejecutamos en ambos sitios, vemos que funciona:
![[Pasted image 20230804091757.png]]
En la máquina windows establecemos el servidor, con el siguiente comando:
```bash
chisel.exe server -p 443
```
![[Pasted image 20230804091913.png]]
Y ahora desde el Kali tenemos que establecer el cliente, de tal forma que lo haremos con la siguiente metodología:
```bash
chisel client <dirección servidor chisel>:<puerto servidor chisel> <puerto local a abrir>:<dirección a donde apuntar>:<puerto a apuntar de la direccion donde se apunta>`
```
El comando es el siguiente:
```bash
./chisel_1.8.1_linux_amd64 client 192.168.0.48:445 5000:192.168.0.30:5000
```
