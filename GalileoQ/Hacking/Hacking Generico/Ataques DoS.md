Para hacer un ataque DoS, podemos utilizar una herramienta que se llama hping3, para paralizar un servidor mandando muchos paquetes TCP:

![[Pasted image 20221217103616.png]]

--icmp: método de ataque
--rand-soure: Para saltarnos el firewall y que se generen IP aleatorias, para no ser detectados.
-d: el tamaño del mensaje (el mensaje de ping máximo es de 1500 bytes, la razón por la que se usa 1400 aquí es porque hay otras cosas en el mensaje de ping).
-Flood: Para enviar los paquetes de la forma más rápida posible.

Y ahora la red estará saturada:

![[Pasted image 20221217103630.png]]

También podemos hacerlo así sobre un puerto en concreto:

![[Pasted image 20221217103639.png]]

Y la máquina se bloquea:

![[Pasted image 20221217103647.png]]

