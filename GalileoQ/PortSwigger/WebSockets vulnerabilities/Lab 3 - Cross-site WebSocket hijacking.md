`TAREA:` Esta tienda en línea tiene una función de chat en vivo implementada mediante WebSockets.

Para resolver el laboratorio, utilice el servidor de exploits para alojar una carga útil HTML/JavaScript que utiliza un ataque de secuestro de WebSocket entre sitios para exfiltrar el historial de chat de la víctima y luego usar esto para obtener acceso a su cuenta.

iniciamos sesión y rápidamente podemos ver que tenemos una cookie de sesión y el parámetro `Same Site` esta configurado como `None`
![[Pasted image 20250729130722.png]]

si analizamos la petición podemos ver que la solicitud del mensaje se envía al servidor haciendo un llamado a recurso ``
![[Pasted image 20250729134949.png]]