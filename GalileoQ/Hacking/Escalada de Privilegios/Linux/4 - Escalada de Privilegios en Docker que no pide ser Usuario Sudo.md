Si nos encontramos con la situación de que podemos ejecutar docker sin ser usuario root, significa que es vulnerable a esta escalada de privilegios:
![[Pasted image 20240612155751.png]]

Por tanto, existe un repositorio de github de hacktricks para escalar privilegios en estos casos:
![[Pasted image 20240612155756.png]]

Y en este repositorio tenemos unas instrucciones de unos comandos que si los ejecutamos en la máquina que puede ejecutar docker sin ser usuario root, nos convertimos en dicho usuario:
![[Pasted image 20240612155803.png]]

Ejecutamos el primer comando:
```python
docker run -it -v /:/host/ ubuntu:18.04 chroot /host/ bash
```
Y ya nos convertimos en usuario root:
![[Pasted image 20240612155807.png]]
