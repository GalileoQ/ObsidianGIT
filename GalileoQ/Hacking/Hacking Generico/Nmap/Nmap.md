**ESCANEOS BÁSICOS CON NMAP**
Si queremos hacer un escaneo básico de los puertos de una máquina podemos hacerlo de esta forma:
![[Pasted image 20230131125255.png]]
**ESCANEAR 2 MÁQUINAS A LA VEZ**
También podemos hacer un escaneo de dos máquinas a la vez con nmap:
![[Pasted image 20230131125457.png]]
**ESCANEAR TODO EL RANGO DE RED**
O también podemos hacer un escaneo completo de nuestra red privada, por lo que detectaremos todos los equipos conectados:
![[Pasted image 20230131125710.png]]
**DESCUIBRIR HOSTS CON NMAP**
Para descubrir hosts con nmap lo haremos así:
```bash
nmap -sn 192.168.0.0/24
```
![[Pasted image 20230726112635.png]]
**ENUMERAR RANGO DE PUERTOS**
También podría enumerar todos los puertos de una máquina o sólo un rango de ellos. Por ejemplo podría enumerar todos de esta forma:
![[Pasted image 20230412010427.png]]
Aunque si ponemos simplemente -p- también estaríamos enumerando todo el rango de puertos:
![[Pasted image 20230412010648.png]]
O también podría analizar sólo un rango de ellos, por ejemplo analizar sólo los 100 primeros:
![[Pasted image 20230412010610.png]]
También podemos enumerar los 500, 100 o el número que queramos del top de los puertos más comunes, por tanto lo haríamos de esta forma:
![[Pasted image 20230412010958.png]]

**ESCANEOS DE NMAP POR UDP (Cuando por TCP no nos encuentra lo que queremos)**

Lo haríamos con el parámetro sU de esta manera escaneando los 100 puertos más comunes:
![[Pasted image 20221217112712.png]]
Y este sería el resultado:
![[Pasted image 20221217112719.png]]
**ESCANEOS DE NMAP A UNA DIRECCIÓN IPV6**
Para hacer un escaneo de puertos a una dirección IPv6 con nmap lo hacemos usando el parámetro -6 de la siguiente forma:
```bash
nmap -p- -sS -sC -sV --min-rate 5000 -n -vvv -Pn -6 fe80::da3a:ddff:fe80:f61c%eth0
```
![[Pasted image 20240107202428.png]]
# PARÁMETROS DE NMAP
**OPCIÓN sP** (Sondeo de Ping)
Esto sirve para detectar equipos conectados a mi red utilizando un ping:
![[Pasted image 20230131130123.png]]
**OPCIÓN -sn**
Este parámetro sirve para detectar equipos encendidos dentro de nuestra web, es algo similar al arp-scan:
![[Pasted image 20230412014503.png]]
**OPCIÓN -sS** (TCP Syn)
Con esta opción podremos hacer un escaneo más sigiloso sin dejar rastro en la máquina objetivo:
![[Pasted image 20230131130250.png]]
**OPCIÓN -T (Plantillas de temporizado)**
El parámetro -T de nmap sirve para aplicar una plantilla para hacer más rápidos o no los escaneos; y tenemos hasta 5 posibles plantillas, siendo la 5 la más rápida.
![[Pasted image 20230412011733.png]]
**OPCIÓN -vvv (Triple verbose)**
Esto sirve para que a medida que encuentre algo, nos lo vaya reportando al mismo tiempo:
![[Pasted image 20230412011332.png]]
**SONDEO UDP**
Para hacer escaneos por UDP utilizamos la opción -sU:
![[Pasted image 20230131130410.png]]
**SONDEO TCP ACK** (Detectar tipo de Firewall con nmap)
Con estas opciones podemos detectar el tipo de firewall:
![[Pasted image 20230131130546.png]]
**SCRIPTS DE NMAP**
Con el script Default ejecutamos los scripts de nmap por defecto para obtener información de las máquinas objetivo:
![[Pasted image 20230131170117.png]]
Por otra parte, tenemos el script vuln podemos brindar información sobre las vulnerabilidades de la víctima:
![[Pasted image 20230131170305.png]]
![[Pasted image 20230131170322.png]]
También podemos ejecutar el script all para ejecutar todos los scripts de nmap:
![[Pasted image 20230131170537.png]]
Y con el script Auth podemos comprobar si existen contraseñas vacías o por defecto en el sistema:
![[Pasted image 20230131170621.png]]
También podemos tener un fuzzer web en nmap con el script HTTP-Enum:
![[Pasted image 20230131170649.png]]
Otro script interesante es para hacer fuerza bruta FTP con nmap de esta forma:
![[Pasted image 20230131170758.png]]
![[Pasted image 20230131170823.png]]
También podemos hacer ataques de fuerza bruta con SSH-Brute:
![[Pasted image 20230131170857.png]]
![[Pasted image 20230131170914.png]]
Para detectar vulnerabilidades en el firewall podemos utilizar este parámetro:
![[Pasted image 20230131171105.png]]

## GUARDAR TRÁFICO DE NMAP EN FICHERO .CAP CON TCPDUMP

Podemos utilizar tcpdump para guardar un determinado tráfico que corre por una interfaz dentro de un fichero con extensión .cap

Lo haríamos de la siguiente forma, donde primero nos ponemos en escucha con tcpdump guardando el tráfico en un fichero y por la interfaz correspondiente:
![[Pasted image 20230412012426.png]]
Y ahora mientras se queda en escucha, vamos a probar por ejemplo en lanzarnos a nosotros mismos un escaneo por el puerto 22 por ejemplo:
![[Pasted image 20230412012708.png]]
Se hace el reporte:
![[Pasted image 20230412012729.png]]
Y ya tenemos el fichero .cap con el tráfico interceptado:
![[Pasted image 20230412012801.png]]
Ahora lanzamos wireshark con la captura:
![[Pasted image 20230412012839.png]]
![[Pasted image 20230412012855.png]]

## ESCANEOS SILENCIOSOS CON NMAP

Para hacer un escaneo de nmap lo más silencioso posible, puede usar la opción "-sS" para realizar un escaneo de "SYN Stealth" en lugar del escaneo de TCP completo. Además, puede utilizar la opción "-T2" para establecer la velocidad de escaneo en un nivel bajo y reducir así el ruido generado por el escaneo en la red.

El siguiente comando de ejemplo utiliza estas opciones para realizar un escaneo de nmap lo más silencioso posible:
```python
nmap -sS -T2 -Pn <dirección_ip>
```
Donde:

-   "-sS" indica que se realizará un escaneo de "SYN Stealth".
-   "-T2" establece la velocidad de escaneo en un nivel bajo.
-   "-Pn" indica que no se realice detección de hosts, lo que reduce aún más el ruido generado por el escaneo.

## NMAP EN WINDOWS (Con Zenmap)
Así sería el asistente de nmap para Windows, el cual se llama zenmap y tiene interfaz gráfica:
![[Pasted image 20230217135652.png]]
También podemos crear un perfil para tener guardado una máquina objetivo con los mismos parámetros de siempre:
![[Pasted image 20230218081525.png]]
![[Pasted image 20230218081748.png]]
Y ya tengo este nuevo perfil con el objetivo y parámetros previamente definidos:
![[Pasted image 20230218081832.png]]
## EVASIÓN DE FIREWALL CON NMAP
Un firewall es un sistema que se encarga de controlar las conexiones entrantes y salientes de una red determinada. Incluso también es posible que un puerto se encuentre como filtrado, lo que significa que hay algo que no está impidiendo conocer con exactitud ese puerto, tal y como nos explican en la páginas man de nmap:
![[Pasted image 20230412193057.png]]
Incluso también, dentro de las páginas man de nmap, tenemos instrucciones para hacer escaneos más silenciosos para evadir los firewall:
![[Pasted image 20230412193242.png]]
**PARÁMETRO -f**
Con el parámetro -f podemos fragmentar los paquetes a la hora de hacer el escaneo, por lo que de esta forma es posible que los puertos que se encuentren filtrados podamos llegar a verlos:
![[Pasted image 20230412193727.png]]
**PARÁMETRO --script-mpu**
El parámetro mpu sirve para ajustar el tamaño máximo de paquete sin fragmentar (MPU) para los paquetes enviados durante las pruebas de detección de firewall. De esta forma podemos evadir firewalls que tengan algún tipo de tope para la detección de este tipo de paquetes. Es importante recalcar que se debe poner siempre un número que sea múltiplo de 8, por ejemplo el 8, 16, etc...
![[Pasted image 20230412194004.png]]
**PARÁMETRO -D (decoy)**
Esto sirve para confundir el firewall. Es decir, sirve para indicar otra dirección IP y hacerle creer al firewall que ademas de lanzar nosotros los paquetes, también lo haga otra dirección IP; y por eso en las instrucciones nos pone que esto sirve para disimular el análisis con señuelos:
![[Pasted image 20230412194342.png]]
Si guardamos esto en un fichero .cap para analizarlo con wireshark, veremos que en este escaneo además de nuestra IP también se habrá enviado otros paquetes desde la IP 192.168.0.232 que hemos proporcionado como señuelo:
![[Pasted image 20230412194600.png]]
![[Pasted image 20230412194623.png]]
[[0 - Consideraciones Previas#TCPDUMP PARA INTERCEPTAR PAQUETES DENTRO DE LA CONEXIÓN]]
**SOURCE PORT (Regular el origen de los paquetes)**

Cuando hacemos un escaneo con nmap se emplea un puerto aleatorio de origen para hacer el escaneo y la respuesta vuelve a entrar por otro puerto aleatorio, tal y como podemos ver en el ejemplo de antes, que se utilizó el puerto 55043:
![[Pasted image 20230413074240.png]]
Es posible que los firewalls tengan una lista blanca de los paquetes que procedan de los puertos más frecuentes en lugar de este tipo de puertos más aleatorios. Lo haríamos de esta forma por ejemplo para indicar que nuestro escaneo procede del puerto 53:
![[Pasted image 20230413074738.png]]
Ahora si examinamos esto con wireshark como antes, veremos que el origen del tráfico ya no procede de un puerto random, sino del puerto 53:
![[Pasted image 20230413074849.png]]
**DATA LENGTH (Modificar la longitud del paquete enviado)**
Es posible que un firewall cuente con restricciones dependiendo del tamaño de paquete. Por lo que podemos modificar este tamaño para engañar al firewall:
![[Pasted image 20230413075148.png]]
Este va a ser el tamaño mínimo que va a tener el paquete, pero le podemos sumar cantidades adicionales de tamaño para estos paquetes con el parámetro --data-length:
![[Pasted image 20230413075434.png]]
Y en wireshark si analizamos este fichero .cap vemos que el tamaño de los paquetes es mayor en un 20:
![[Pasted image 20230413075730.png]]
**CAMBIAR DIRECCIÓN MAC EN NMAP**
Con la opción --spoof-mac podremos cambiar la MAC:
![[Pasted image 20230131165550.png]]
# SCRIPTS DE NMAP
Nmap cuenta con scripts para automatizar ciertas partes del reconocimiento; y para ello podemos utilizar el parámetro -sC para lanzar ciertos scripts de nmap. Por lo que si además lo juntamos con el parámetro -sV obtendremos la versión de los servicios del puerto:
![[Pasted image 20230413081220.png]]
También podemos lanzar un script en concreto, por ejemplo el script vuln para detectar vulnerabilidades de un puerto en concreto:
![[Pasted image 20230413081456.png]]
Aunque por sí solo el script vuln es muy intrusivo, por lo que podemos lanzar el script safe junto con el vuln:
![[Pasted image 20230413081949.png]]
También tenemos otro script para hacer fuzzing web muy sencillo con el script llamado http-enum:
![[Pasted image 20230413082307.png]]
## COMO HACER UN ESCANEO DE NMAP MUCHO MÁS RÁPIDO Y EFICIENTE

• Ponemos triple verbose para que se nos vaya mostrando los resultados mientras analiza.

◇ Con -n evitamos la resolución DNS

◇ Con min rate también aceleramos.

◇ Con el comando -Pn indicamos que no realice una comprobación de ping antes de realizar el escaneo de puertos.

◇ Con -oG lo podemos exportar.

```bash
nmap -p- --open -sS -sC -sV --min-rate 5000 -vvv -n -Pn 10.10.11.191 -oN escaneo
```
