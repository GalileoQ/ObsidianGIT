Tarea: La función de cambio de correo electrónico de este laboratorio es vulnerable a ataques CSRF. Utiliza tokens para intentar prevenirlos, pero no están integrados en el sistema de gestión de sesiones del sitio.

Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.

Tienes dos cuentas en la aplicación que puedes usar para diseñar tu ataque. Las credenciales son las siguientes:

- `wiener:peter`
- `carlos:montoya`

ingresamos en el laboratorio como el usuario `wiener` y vamos a actualizar el correo electronico
![[Pasted image 20250704160613.png]]

vamos a enviar la solicitud al Repeater para almacenarla y hacemos `Drop` de la petición para que esta no se complete
![[Pasted image 20250704160718.png]]

ahora vamos a iniciar sesión como el usuario `carlos` y vamos a actualizar el correo interceptando la petición de nuevo
![[Pasted image 20250704161023.png]]

