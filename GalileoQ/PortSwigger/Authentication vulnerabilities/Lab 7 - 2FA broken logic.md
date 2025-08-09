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

ahora si vemos la solicitud `GET` que se hace en el inicio de sesión con el código de wiener podemos ver que tenemos el parámetro `verify` que verifica que este código es del usuario `wiener` lo que podemos hacer es cambiar el usuario por carlos y tratar de conseguir un código de verificación para carlos 
![[Pasted image 20250809115329.png]]

en este caso específicamente nos interesa la solicitud `POST /login2` ya que esta contiene la verificación del usuario y el código. los parámetros que podemos modificar para intentar acceder como el usuario carlos
![[Pasted image 20250809115831.png]]

enviamos nuestra solicitud al repeater. cambiamos al usuario por carlos que en este caso es nuestra victima. tambien t
![[Pasted image 20250809120101.png]]