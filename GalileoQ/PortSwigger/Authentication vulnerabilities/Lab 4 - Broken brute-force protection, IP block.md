`TAREA:` Este laboratorio es vulnerable debido a una falla lógica en su protección de contraseñas por fuerza bruta. Para solucionarlo, se debe forzar la contraseña de la víctima, iniciar sesión y acceder a la página de su cuenta.

- Sus credenciales: `wiener:peter`
- Nombre de usuario de la víctima: `carlos`
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

intentamos iniciar sesión con la cuenta de carlos y enviamos la solicitud al repeater. nos damos cuenta que la web solo te deja enviar 2 veces la solicitud antes de darte el mensaje `You have made too many incorrect login attempts. Please try again in 1 minute(s).` por lo que tenemos que tener esto en consideración 
![[Pasted image 20250805154459.png]]

haciendo una prueba podemos ver que solo nos permite 2 intentos inválidos así que debemos jugar con esta característica 
![[Pasted image 20250807120137.png]]

cuando hacemos una solicitud con las credenciales correctas nos da un `Status code 302` es un estado correcto pero nos hace una redirección
![[Pasted image 20250807120237.png]]

