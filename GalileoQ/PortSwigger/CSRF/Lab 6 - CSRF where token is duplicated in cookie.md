`TAREA:`
Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.

Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

como siempre iniciamos sesión en el lab con las credenciales que nos proporcionan e interceptamos la solicitud para luego enviarla al repeater
![[Pasted image 20250710162852.png]]

después de interceptar la solicitud la enviamos al repeater y podemos tirar (Drop) la intercepción para no realizar ningún cambio 

podemos ver que tenemos tanto la `cookie` de inicio de sesión como el `csrf`
![[Pasted image 20250710163229.png]]

