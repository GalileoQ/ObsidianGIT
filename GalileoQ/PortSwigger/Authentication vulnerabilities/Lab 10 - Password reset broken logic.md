La función de restablecimiento de contraseña de este laboratorio es vulnerable. Para solucionar el problema, restablezca la contraseña de Carlos, inicie sesión y acceda a su página "Mi cuenta".

- Sus credenciales: `wiener:peter`
- Nombre de usuario de la víctima: `carlos`

iniciamos sesión con las credenciales que nos da el laboratorio y tenemos un correo electrónico y también podemos ir al `email client` y tenemos la opción de `update email`
![[Pasted image 20250811130651.png]]

ahora lo importante aquí es el parámetro de `Forgot passord` el cual nos permite reestablecer la contraseña del usuario y esta es la tarea que debemos cumplir
![[Pasted image 20250811131049.png]]

como no tenemos acceso al servidor de correo electrónico de carlos no podemos realizar esta tarea así que debemos usar las solicitudes del usuario wiener
![[Pasted image 20250811131250.png]]

capturamos la solicitud del usuario wiener y la enviamos al repeater para analizarla
![[Pasted image 20250811131528.png]]

por otro lado la web nos indica que debemos revisar el correo para 
![[Pasted image 20250811131722.png]]

el servidor de correos nos da el link donde podemos hacer el cambio de contraseña
![[Pasted image 20250811132237.png]]

ahora lo importnate de esto es que podemos ver una solicitud `POST` donde se pueden 
![[Pasted image 20250811132331.png]]