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