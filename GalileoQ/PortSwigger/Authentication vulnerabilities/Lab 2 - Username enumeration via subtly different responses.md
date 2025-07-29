`TAREA:` Este laboratorio es sutilmente vulnerable a ataques de fuerza bruta de enumeración de nombres de usuario y contraseñas. Tiene una cuenta con un nombre de usuario y una contraseña predecibles, que se pueden encontrar en las siguientes listas de palabras:

- [Nombres de usuario de los candidatos](https://portswigger.net/web-security/authentication/auth-lab-usernames)
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

Para resolver el laboratorio, enumere un nombre de usuario válido, fuerce la contraseña de este usuario y luego acceda a su página de cuenta.

tratamos de iniciar sesión y capturamos la solicitud. igual que siempre.
![[Pasted image 20250729154926.png]]

ahora. para este ataque haremos exactamente lo mismo. una fuerza bruta de usuarios pero debemos indicar que haga referencia al error `Invalid username or password` esto lo que hará será un ataque de fuerza bruta de usuarios teniendo en cuenta el mensaje de respuesta y si algo cambia en este mensaje de respuesta podremos notarlo
![[Pasted image 20250729160325.png]]

