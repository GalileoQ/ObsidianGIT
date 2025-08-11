`TAREA:` Este laboratorio es vulnerable al envenenamiento por restablecimiento de contraseña. El usuario `carlos`Hará clic sin pensar en cualquier enlace de los correos electrónicos que reciba. Para resolver el laboratorio, inicie sesión en la cuenta de Carlos. Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`Cualquier correo electrónico enviado a esta cuenta puede leerse a través del cliente de correo electrónico en el servidor de exploits.

vamos a realizar un cambio de contraseñas nuevamente para el usuario calros, primero debemos iniciar sesión con las credenciales de wiener y analizar la solicitud
![[Pasted image 20250811140353.png]]

bien. al realizar un cambio de contraseña para el usuario `wiener` podemos ver que tenemos 3 solicitudes diferentes que son las encargadas de reestablecer la contraseña. 

una solicitud `POST` principal una solicitud `GET` encargada de enviarnos el link que hemos recibido al correo y nuevamente una solicitud `POST` la cual contiene los datos de la nueva contraseña. 
![[Pasted image 20250811143839.png]]

ahora bien. lo primero que tenemos que hacer es modificar la solicitud y enviarla con el usuario carlos. agregando el parámetro `X-Forwarded-Host` para definir el host hacia donde se enviara. en este caso vamos a usar nuestro servidor de exploit para poder ver la respuesta
![[Pasted image 20250811153550.png]]

de esta manera una ves que el usuario carlos haga clic en el enlace esto generara una respuesta que contiene la cookie temporal para reestablecer la contraseña
![[Pasted image 20250811153936.png]]

ahora en la solicitud `GET` vamos a reemplazar 
![[Pasted image 20250811154022.png]]