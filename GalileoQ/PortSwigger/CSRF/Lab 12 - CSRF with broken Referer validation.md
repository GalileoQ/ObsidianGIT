`TAREA:` La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Intenta detectar y bloquear solicitudes entre dominios, pero el mecanismo de detección puede eludirse. Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.

Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

iniciamos sesión en el laboratorio interceptamos la solicitud de cambio de contraseña y la enviamos al repeater `history.pushState('', '', '/');` el primer valor hace referencia al estatus el segundo valor es el titulo y el tercero es la URL. en este caso nos interesa el de la url. 

`NOTA:` la URL puede ser relativa o absoluta. pero debe 
![[Pasted image 20250722123446.png]]

en este caso lo primero que hacemos es validar cual es la porción del referer que se esta validando. hasta que encontramos una respuesta `400 Bad Request` esto nos indica que la solicitud ya no se esta validando correctamente por lo que podemos identificar hasta donde llega la validación del referer.
![[Pasted image 20250722123602.png]]

creamos nuestro CSRF-POC y podemos usar el parámetro 
![[Pasted image 20250722125434.png]]