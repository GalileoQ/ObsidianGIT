# Con Autoroute y Módulo arp_scanner en Windows
Estamos dentro de la máquina intermedia:
![[Pasted image 20230729113039.png]]
Detectamos la interfaz de red objetivo:
![[Pasted image 20230729113134.png]]
Aplicamos el autoroute:
![[Pasted image 20230729113905.png]]
Ahora usamos el módulo de arp_scanner:
```bash
use post/windows/gather/arp_scanner
```
![[Pasted image 20230729114843.png]]
Y vemos que efectivamente la IP 10.10.10.11 es el objetivo:
![[Pasted image 20230729114929.png]]
# Con Portscan/tcp en Linux
Una vez configurado y ejecutado el autoroute, usamos este módulo:
```bash
use auxiliary/scanner/portscan/tcp
```
Donde debemos de configurar los puertos que queramos escanear:
![[Pasted image 20230729113232.png]]
![[Pasted image 20230729113431.png]]
Y nos encuentra resultados:
![[Pasted image 20230729115201.png]]
Y si queremos buscar de varias IPs diferentes y encontrar equipos dentro de la red, lo haríamos así:
![[Pasted image 20230729125241.png]]