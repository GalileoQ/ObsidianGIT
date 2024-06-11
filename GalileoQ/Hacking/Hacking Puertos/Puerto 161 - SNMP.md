Antes de nada, hay que entender qué es el protocolo snmp:

- (SNMP) es un protocolo de capa de aplicación que facilita el intercambio de información de administración entre dispositivos de red
- La herramienta Onesixtyone es una herramienta de administración de red basada en SNMP que se utiliza para recopilar y analizar datos de dispositivos de red.
- En el contexto de SNMP, una community key es una cadena de caracteres que se utiliza para autenticar el acceso a un dispositivo de red.
Si buscamos en internet cómo enumerar el servicio snmp, tenemos una herramienta llamada onesixtyone usando el siguiente diccionario:
```
https://github.com/hackingyseguridad/diccionarios/blob/master/usuarios.txt
```
![[Pasted image 20231120192239.png]]
Este diccionario lo utilizaremos con onesixtyone de la siguiente forma:
```
onesixtyone -c usuarios.txt 192.168.0.25
```
Y vemos que nos encuentra la community key llamada security. La cadena de comunidad actúa como una especie de contraseña que permite a los dispositivos de gestión SNMP comunicarse con los agentes SNMP en los dispositivos gestionados: 
![[Pasted image 20231120192506.png]]
Ahora con esta contraseña, podemos enumerar este protocolo con snmpwalk, donde utilizaremos el siguiente comando que utilizará la versión 2c y la cadena de comunidad security:
```
snmpwalk -v 2c -c security 192.168.0.25
```
Y nos encontramos un dominio:
![[Pasted image 20231120192822.png]]
Que lo