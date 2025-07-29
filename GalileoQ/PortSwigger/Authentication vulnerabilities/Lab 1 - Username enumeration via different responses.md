`Tarea:` Este laboratorio es vulnerable a ataques de fuerza bruta de enumeración de nombres de usuario y contraseñas. Tiene una cuenta con un nombre de usuario y una contraseña predecibles, que se pueden encontrar en las siguientes listas de palabras:

- [Nombres de usuario de los candidatos](https://portswigger.net/web-security/authentication/auth-lab-usernames)
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

Para resolver el laboratorio, enumere un nombre de usuario válido, fuerce la contraseña de este usuario y luego acceda a su página de cuenta.

vamos a iniciar sesión y debemos interceptar esta solicitud ya que es la que contiene el `login`
![[Pasted image 20250729152332.png]]

para realizar la enumeración de usuarios debemos enviar la solicitud al `Intruder` luego realizar un ataque de fuerza bruta. para esto necesitamos usar el 
![[Pasted image 20250729152923.png]]