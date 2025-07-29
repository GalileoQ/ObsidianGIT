`Tarea:` Este laboratorio es vulnerable a ataques de fuerza bruta de enumeración de nombres de usuario y contraseñas. Tiene una cuenta con un nombre de usuario y una contraseña predecibles, que se pueden encontrar en las siguientes listas de palabras:

- [Nombres de usuario de los candidatos](https://portswigger.net/web-security/authentication/auth-lab-usernames)
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

Para resolver el laboratorio, enumere un nombre de usuario válido, fuerce la contraseña de este usuario y luego acceda a su página de cuenta.

vamos a iniciar sesión y debemos interceptar esta solicitud ya que es la que contiene el `login`
![[Pasted image 20250729152332.png]]

para realizar la enumeración de usuarios debemos enviar la solicitud al `Intruder` luego realizar un ataque de fuerza bruta. para esto necesitamos usar el signo que parece una serpiente doble o algo así. esta en el botón de `add` luego cargamos nuestra lista de usuarios y realizamos el ataque. 

demos que todos los usuarios tienen un `Status code 200` pero si analizamos el `Length` tenemos uno que es diferente. y si vemos el `Response` indica que la contraseña es incorrecta. y si seleccionamos un usuario diferente tendremos una respuesta de usuario incorrecto. así que ya tenemos el usuario. vamos por la contraseña
![[Pasted image 20250729152923.png]]

realizamos el mismo ataque para la contraseña y de igual forma encontramos una que tiene un `Length` muy diferente a los demas
![[Pasted image 20250729154544.png]]

probamos las contraseñas y podemos iniciar sesión en el laboratorio
![[Pasted image 20250729154449.png]]