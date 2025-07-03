Tarea: La funcionalidad de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico de la victima. Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

interceptamos la solicitud y la enviamos al repeater. de esta manera podemos analizar la solicitud  
![[Pasted image 20250703174201.png]]

si eliminamos el token la respuesta nos indica que falta el parámetro `csrf` 
![[Pasted image 20250703174558.png]]

si eliminamos los parámetros `email` y `csrf` la respuesta nos indica que falta el parámetro email 
![[Pasted image 20250703174652.png]]

ok. parece que el parámetro `csrf` solo tiene una validación si este se encuentra presente. por lo que podemos simplemente eliminar este 