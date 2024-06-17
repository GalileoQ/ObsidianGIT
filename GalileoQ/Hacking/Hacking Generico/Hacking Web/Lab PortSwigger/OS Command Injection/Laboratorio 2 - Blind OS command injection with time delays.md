Supongamos que tenemos este panel de rellenar información:
![[Pasted image 20240613005746.png]]

Lo que hay que hacer es interceptar esta petición con burp suite y fijarnos en la parte donde se está enviando la información:
![[Pasted image 20240613005752.png]]

En este punto podemos concatenar un operador lógico para insertar un comando, por ejemplo en el campo de email podemos poner ||, ya que es el operador lógico or; y ahí concatenar un comando, por ejemplo el comando ping, por lo que lo url encodeamos:
![[Pasted image 20240613005801.png]]

Y lo pegamos en la parte donde se transmite la información y con los operadores lógicos:
![[Pasted image 20240613005804.png]]

Ahora esperamos los 10 segundos y vemos que es lo que tarda la página en enviar los datos, porque se está ejecutando primero las 10 trazas ICMP, por lo que tenemos ejecución remota de comandos.

