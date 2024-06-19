#ActiveDirectory 

Podemos conectarnos a recursos compartidos de una máquina windows con esta herramienta, por ejemplo de la siguiente forma:

```python
rpcclient -U "" 192.168.0.136
```

![[Pasted image 20240615210419.png]]

Y aquí tenemos una serie de comando útiles para extraer información:
### COMANDO HELP

![[Pasted image 20240615210432.png]]
### COMANDO SRVINFO
Nos muestra información del sistema:
![[Pasted image 20240615210445.png]]
### ENUMERAR GRUPOS Y USUARIOS

![[Pasted image 20240615210502.png]]

![[Pasted image 20240615210507.png]]

# ENUM4LINUX
Podemos también hacer un análisis de la máquina windows a través del puerto 445 con esta herramienta, por lo que usamos el siguiente comando:
```python
enum4linux 192.168.171.136 -A
```
