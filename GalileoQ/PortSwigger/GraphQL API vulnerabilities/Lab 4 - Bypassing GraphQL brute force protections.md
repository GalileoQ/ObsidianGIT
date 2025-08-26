`TAREA:` El mecanismo de inicio de sesión de usuario para este laboratorio se basa en una API GraphQL. El punto final de la API tiene un limitador de velocidad que devuelve un error si recibe demasiadas solicitudes del mismo origen en poco tiempo.

Para resolver el laboratorio, fuerce el mecanismo de inicio de sesión para iniciar sesión como `carlos`Utilice la lista de contraseñas del laboratorio de autenticación como fuente de contraseñas.

Obtenga más información sobre cómo trabajar con GraphQL en Burp Suite.

intentamos iniciar sesión para interceptar esta solicitud y analizar el flujo de trabajo de la web
![[Pasted image 20250826094546.png]]

la solicitud nos muestra un estatus `200 OK` y tenemos un parámetro `token` y también un parámetro `success` que en este caso es falso
![[Pasted image 20250826094820.png]]

al analizar la solicitud podemos ver que después de enviarla mas de  2 veces se aplica un bloqueo de 1 minuto donde tenemos el mensaje `"You have made too many incorrect login attempts. Please try again in 1 minute(s)."`  
![[Pasted image 20250826094710.png]]

si analizamos el `GraphQL` podemos ver la query que se realiza para el inicio de sesión pero esta query no parece tener nada que la proteja así que vamos a intentar modificarla
![[Pasted image 20250826095829.png]]

para modificar la query debemos s
![[Pasted image 20250826100713.png]]