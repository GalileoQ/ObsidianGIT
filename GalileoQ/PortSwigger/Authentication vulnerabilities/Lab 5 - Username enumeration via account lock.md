`TAREA:` Este laboratorio es vulnerable a la enumeración de nombres de usuario. Utiliza el bloqueo de cuentas, pero esto contiene una falla lógica. Para solucionar el problema, enumere un nombre de usuario válido, use fuerza bruta para obtener su contraseña y acceda a su página de cuenta.

- [Nombres de usuario de los candidatos](https://portswigger.net/web-security/authentication/auth-lab-usernames)
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

primero vamos a intentar iniciar sesión y vamos a capturar la solicitud en el repeater podemos ver que efectivamente nuestras credenciales son invalidads
![[Pasted image 20250807134000.png]]

enviamos la petición al intruder y vamos a realizar un ataque tipo `Cluster bomb attack` y hacemos un ataque al usuario y un ataque a la contraseña. es importante lo hagas al lado de la contraseña ya que será un ataque `null`
![[Pasted image 20250807134115.png]]

ahora vamos a agregar una opción extra. para ello vamos a `settings` > `Grep - Extract` > `Add` y vamos a agregar el mensaje de `Invalid username or password` 

`NOTA:` el segundo payload debes identificarlo como null payload. en la siguiente imagen puedes ver un ejemplo.
![[Pasted image 20250807134535.png]]


![[Pasted image 20250807134946.png]]