Tarea: La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Intenta bloquear ataques CSRF, pero solo aplica defensas a ciertos tipos de solicitudes.
Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.
Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales:`wiener:peter`

iniciamos el laboratorio con las credenciales. vamos a interceptar la solicitud de `Update email` para analizar su funcionamiento
![[Pasted image 20250703135512.png]]

podemos ver la solicitud e identificar que esta tiene un token CSRF. este token 
![[Pasted image 20250703135653.png]]