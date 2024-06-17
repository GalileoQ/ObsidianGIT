en este laboratorios vamos a realizar pruebas de pivoting y Tunneling entre 3 maquinas synfonos (1,2,3) 

primero vamos a identificar nuestra interfaz de red para saber en que segmento estamos operando y luego vamos a realizar un escaneo de nuestra red para identificar posibles maquinas que estén activas

```python
	ifconfig

	sudo apr-scan -I eth0 --localnet

	netdiscover -i eth0 -r ip
```

![[Pasted image 20240617144232.png]]