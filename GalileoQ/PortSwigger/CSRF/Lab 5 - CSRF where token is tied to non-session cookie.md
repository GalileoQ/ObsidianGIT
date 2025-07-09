`Tarea:`
Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.

Tienes dos cuentas en la aplicación que puedes usar para diseñar tu ataque. Las credenciales son las siguientes:

- `wiener:peter`
- `carlos:montoya`

iniciamos sesión en el laboratorio con las credenciales de `wiener` intentamos actualizar el correo y capturamos la petición. 

podemos ver un parámetro llamado `csrf` que es nuestro token y un segundo parámetro llamado `csrfKey` enviamos esta información al repeater y vamos a iniciar con el usuario `carlos` pare ver cuales son las diferencias
![[Pasted image 20250709132926.png]]

en este caso podemos ver los mismos parámetros 
![[Pasted image 20250709133409.png]]