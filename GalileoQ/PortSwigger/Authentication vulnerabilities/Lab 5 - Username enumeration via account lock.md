`TAREA:` Este laboratorio es vulnerable a la enumeración de nombres de usuario. Utiliza el bloqueo de cuentas, pero esto contiene una falla lógica. Para solucionar el problema, enumere un nombre de usuario válido, use fuerza bruta para obtener su contraseña y acceda a su página de cuenta.

- [Nombres de usuario de los candidatos](https://portswigger.net/web-security/authentication/auth-lab-usernames)
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

primero vamos a intentar iniciar sesión y vamos a capturar la solicitud en el repeater podemos ver que efectivamente nuestras credenciales son invalidads
![[Pasted image 20250807134000.png]]

