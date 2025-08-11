`TAREA:` Este laboratorio permite a los usuarios mantener la sesión iniciada incluso después de cerrar el navegador. La cookie utilizada para proporcionar esta funcionalidad es vulnerable a ataques de fuerza bruta.

Para resolver el laboratorio, fuerza brutamente la cookie de Carlos para obtener acceso a su **página Mi cuenta** .

- Sus credenciales: `wiener:peter`
- Nombre de usuario de la víctima: `carlos`
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

primero vamos a iniciar sesión con las credenciales que nos han proporcionado y lo importante aquí es marcar la opción de `Stay logged in`
![[Pasted image 20250811114612.png]]

capturamos esta solicitud y la enviamos al repeater. podemos ver que tenemos una cookie de sesión donde esta el parámetro `Stay-logget-in` así que vamos a decodear esta cookie para ver que información nos da.
![[Pasted image 20250811115235.png]]

esta cookie es el nombre del usuario que inicia sesión y un código que parece ser MD5
![[Pasted image 20250811115525.png]]

confirmamos que es un hash MD5 y básicamente es la contraseña del usuario
![[Pasted image 20250811115739.png]]

