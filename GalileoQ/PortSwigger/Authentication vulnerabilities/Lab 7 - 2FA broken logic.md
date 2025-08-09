`TAREA:` La autenticación de dos factores de este laboratorio es vulnerable debido a su lógica defectuosa. Para solucionar el problema, acceda a la página de la cuenta de Carlos.

- Sus credenciales: `wiener:peter`
- Nombre de usuario de la víctima: `carlos`

También tienes acceso al servidor de correo electrónico para recibir tu código de verificación 2FA.

iniciamos sesión con las credenciales de `wiener` y necesitamos un código de 4 dígitos para realizar la autenticación en dos pasos. para ello podemos ir a `Email client` que sera nuestro cliente de correo donde recibiremos el código 
![[Pasted image 20250809115030.png]]

tenemos nuestro código y podemos verificarlo
![[Pasted image 20250809115200.png]]

efectivamente iniciamos sesión con las credenciales de `wiener`
![[Pasted image 20250809115231.png]]

ahora si vemos la solicitud `GET` que se hace en el inicio 
![[Pasted image 20250809115329.png]]