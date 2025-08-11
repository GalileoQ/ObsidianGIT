`TAREA:` Este laboratorio es vulnerable al envenenamiento por restablecimiento de contraseña. El usuario `carlos`Hará clic sin pensar en cualquier enlace de los correos electrónicos que reciba. Para resolver el laboratorio, inicie sesión en la cuenta de Carlos. Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`Cualquier correo electrónico enviado a esta cuenta puede leerse a través del cliente de correo electrónico en el servidor de exploits.

vamos a realizar un cambio de contraseñas nuevamente para el usuario calros, primero debemos iniciar sesión con las credenciales de wiener y analizar la solicitud
![[Pasted image 20250811140353.png]]

bien. al realizar un cambio de contraseña para el usuario `wiener` podemos ver que tenemos 3 solicitudes diferentes que son las encargadas de reestablecer la contraseña. 

una solicitud `POST` principal una solicitud `GET` encargada de enviarnos el link que hemos recibido al correo y nuevamente una solicitud `POST` la cual contiene los datos de la nueva contraseña. 
![[Pasted image 20250811143839.png]]