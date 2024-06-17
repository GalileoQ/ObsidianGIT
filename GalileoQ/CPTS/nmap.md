```python
|**Opción Nmap**|**Descripción**|
|---|---|
|`10.10.10.0/24`|Rango de red objetivo.|
|`-sn`|Desactiva el escaneo de puertos.|
|`-sT`|Escanear Puertos TCP.|
|`-Pn`|Deshabilita las solicitudes de eco ICMP|
|`-n`|Desactiva la resolución DNS.|
|`-PE`|Realiza el escaneo de ping mediante solicitudes de eco ICMP contra el objetivo.|
|`--packet-trace`|Muestra todos los paquetes enviados y recibidos.|
|`--reason`|Muestra el motivo de un resultado específico.|
|`--disable-arp-ping`|Deshabilita las solicitudes de ping ARP.|
|`--top-ports=<num>`|Explora los puertos principales especificados que se han definido como más frecuentes.|
|`-p-`|Escanee todos los puertos.|
|`-p22-110`|Escanee todos los puertos entre 22 y 110.|
|`-p22,25`|Explora sólo los puertos especificados 22 y 25.|
|`-F`|Escanea los 100 puertos principales.|
|`-sS`|Realiza un TCP SYN-Scan.|
|`-sA`|Realiza un escaneo TCP ACK.|
|`-sU`|Realiza un escaneo UDP.|
|`-sV`|Analiza los servicios descubiertos en busca de sus versiones.|
|`-sC`|Realice un análisis de secuencias de comandos con secuencias de comandos categorizadas como "predeterminadas".|
|`--script <script>`|Realiza un análisis de secuencias de comandos utilizando las secuencias de comandos especificadas.|
|`-O`|Realiza un análisis de detección del sistema operativo para determinar el sistema operativo del objetivo.|
|`-A`|Realiza detección de sistema operativo, detección de servicios y análisis de traceroute.|
|`-D RND:5`|Establece el número de señuelos aleatorios que se utilizarán para escanear el objetivo.|
|`-e`|Especifica la interfaz de red que se utiliza para el análisis.|
|`-S 10.10.10.200`|Especifica la dirección IP de origen para el análisis.|
|`-g`|Especifica el puerto de origen para el análisis.|
|`--dns-server <ns>`|La resolución de DNS se realiza utilizando un servidor de nombres específico.|

## Opciones de salida

|**Opción Nmap**|**Descripción**|
|---|---|
|`-oA filename`|Almacena los resultados en todos los formatos disponibles comenzando con el nombre de "nombre de archivo".|
|`-oN filename`|Almacena los resultados en formato normal con el nombre "nombre de archivo".|
|`-oG filename`|Almacena los resultados en formato "grepable" con el nombre de "nombre de archivo".|
|`-oX filename`|Almacena los resultados en formato XML con el nombre de "nombre de archivo".|

## Opciones de desempeño

|**Opción Nmap**|**Descripción**|
|---|---|
|`--max-retries <num>`|Establece el número de reintentos para escaneos de puertos específicos.|
|`--stats-every=5s`|Muestra el estado del escaneo cada 5 segundos.|
|`-v/-vv`|Muestra resultados detallados durante el escaneo.|
|`--initial-rtt-timeout 50ms`|Establece el valor de tiempo especificado como tiempo de espera inicial de RTT.|
|`--max-rtt-timeout 100ms`|Establece el valor de tiempo especificado como tiempo de espera máximo de RTT.|
|`--min-rate 300`|Establece el número de paquetes que se enviarán simultáneamente.|
|`-T <0-5>`|Especifica la plantilla de sincronización específica.|
```

### nmap comandos
```python
sudo nmap -sSU -p 53 --script dns-nsid 10.129.142.139 | # enumerando version del servidor DNS


```