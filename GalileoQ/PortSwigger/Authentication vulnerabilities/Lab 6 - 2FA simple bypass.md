`TAREA:` La autenticación de dos factores de este laboratorio se puede omitir. Ya tienes un nombre de usuario y una contraseña válidos, pero no tienes acceso al código de verificación de dos factores del usuario. Para resolver el laboratorio, accede a la página de la cuenta de Carlos.

- Sus credenciales: `wiener:peter`
- Credenciales de la víctima `carlos:montoya`

iniciamos sesión con las credenciales de `wiener` aqui podemos ver que el inicio de sesión tiene 2 solicitudes diferentes una solicitud `POST /login` y otra `GET /login2` 
![[Pasted image 20250807140527.png]]

la solicitud `GET /login2` vemos que no tiene ningún tipo de restricción y además el parámetro `X-Frame-Options: SAMEORIGIN` esta seteado así que esto esta haciendo una verificación de 2 pasos pero no tiene ninguna restricción o key que la este protegiendo
![[Pasted image 20250807140745.png]]

vamos a iniciar sesión con las credenciales de `carlos` y vamos a enviar la solicitud haciendo clic en `Forward`
![[Pasted image 20250807141111.png]]

