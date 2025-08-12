La función de cambio de contraseña de este laboratorio lo hace vulnerable a ataques de fuerza bruta. Para solucionar el problema, use la lista de contraseñas candidatas para forzar la cuenta de Carlos y acceder a su página "Mi cuenta".

- Sus credenciales: `wiener:peter`
- Nombre de usuario de la víctima: `carlos`
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

iniciamos sesión con las credenciales que nos proporciona el laboratorio y detectamos el proceso para el cambio de contraseña. debemos proporcionar la contraseña actual y luego la nueva contraseña y después confirmarla. así que vamos a interceptar esta solicitud
![[Pasted image 20250812114608.png]]

la solicitud de cambio de contraseña se ve así. contiene los datos que hemos enviado y hace una solicitud `POST` a `/my-account/change-password`

`NOTA:` el comportamiento al ingresar la contraseña actual incorrecta. Si las dos entradas de la nueva contraseña coinciden, la cuenta se bloquea. Sin embargo, si ingresa dos contraseñas nuevas diferentes, un mensaje de error simplemente indica `Current password is incorrect`Si ingresa una contraseña actual válida, pero dos contraseñas nuevas diferentes, el mensaje dice `New passwords do not match`Podemos usar este mensaje para enumerar las contraseñas correctas.
![[Pasted image 20250812114816.png]]

sabiendo esto podemos enumerar una contraseña para el usuario carlos. simplemente con una contraseña incorrecta a la cual le aplicaremos el ataque de fuerza bruta y un par de contraseñas que no coincidan para que nuestra cuenta no sea bloquee ya que estaríamos cayendo en uno de los pasos que hemos comentado anteriormente.
![[Pasted image 20250812120341.png]]

despues simplemente buscamos e
![[Pasted image 20250812120303.png]]