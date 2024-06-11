Podemos conectarnos a recursos compartidos de una máquina windows con esta herramienta, por ejemplo de la siguiente forma:
```bash
rpcclient -U "" 192.168.0.136
```
![[Pasted image 20230729131319.png]]
Y aquí tenemos una serie de comando útiles para extraer información:
## COMANDO HELP
![[Pasted image 20230729131354.png]]
# COMANDO SRVINFO
Nos muestra información del sistema:
![[Pasted image 20230729131514.png]]
# ENUMERAR GRUPOS Y USUARIOS
![[Pasted image 20230729131539.png]]
![[Pasted image 20230729131612.png]]
# ENUM4LINUX
Podemos también hacer un análisis de la máquina windows a través del puerto 445 con esta herramienta, por lo que usamos el siguiente comando:
```
enum4linux 192.168.171.136 -A
```
