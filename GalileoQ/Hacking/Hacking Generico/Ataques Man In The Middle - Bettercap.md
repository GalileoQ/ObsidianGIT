Vamos a utilizar la herramienta de bettercap:
![[Pasted image 20240612162946.png]]
## ARP Spoofing
Se trata del primer ataque que tenemos que efectuar; y el cual consiste en que un atacante manipula la tabla de direcciones de resolución de protocolo (ARP) de una red local. El ARP es un protocolo utilizado para asociar direcciones IP con direcciones MAC (direcciones físicas) en una red.

-----------

Vemos que la máquina windows víctima tiene la siguiente tabla ARP, la cual es normal:
![[Pasted image 20240612162952.png]]

Y desde el Kali, iniciamos el ataque ARP Spoofing de la siguiente forma:
```bash
net.probe on
set arp.spoof.targets 192.168.0.21 # IP de la máquina Windows víctima.
arp.spoof on
```
![[Pasted image 20240612163000.png]]

Y ahora, si ejecutamos el comando arp -a desde windows, veremos que la dirección MAC de la puerta de enlace ahora coincide con la de nuestra máquina atacante:
![[Pasted image 20240612163006.png]]

![[Pasted image 20240612163010.png]]

Esto implica que ahora la máquina víctima al navegar por la red, nosotros podremos ver todo su tráfico:
![[Pasted image 20240612163016.png]]

Y desde el Kali con wireshark lo vemos todo:
![[Pasted image 20240612163023.png]]

# DNS Spoofing
DNS Cache Poisoning, es una técnica maliciosa en la que un atacante intenta introducir información falsa en la caché del sistema DNS (Domain Name System). Es decir, el atacante puede interceptar o falsificar las respuestas de un servidor DNS para engañar a la víctima y dirigirla a un sitio web malicioso en lugar del sitio web legítimo al que intenta acceder.

-------------------

Tenemos que poner una dirección falsa y luego la IP de la máquina atacante donde queramos redirigir a la víctima, por lo que primero levantamos una web en nuestro localhost de atacante:
![[Pasted image 20240612163038.png]]

Y a continuación, hacemos el dns spoofing:
```bash
set dns.spoof.domains facebook.es
set dns.spoof.address 192.168.0.24
```
![[Pasted image 20240612163036.png]]
Y luego ejecutamos el comando:
```bash
dns.spoof on
```

![[Pasted image 20240612163047.png]]
Y ahora si visitamos facebook.es desde la víctima, nos habrá redirigido al puerto 80 de nuestra máquina atacante:
![[Pasted image 20240612163049.png]]
