La función de cambio de contraseña de este laboratorio lo hace vulnerable a ataques de fuerza bruta. Para solucionar el problema, use la lista de contraseñas candidatas para forzar la cuenta de Carlos y acceder a su página "Mi cuenta".

- Sus credenciales: `wiener:peter`
- Nombre de usuario de la víctima: `carlos`
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

iniciamos sesión con las credenciales que nos proporciona el laboratorio y detectamos el proceso para el cambio de contraseña. debemos proporcionar la contraseña actual y luego la nueva contraseña y después confirmarla. así que vamos a interceptar esta solicitud
![[Pasted image 20250812114608.png]]

la solicitud de cambio de contraseña se ve así. contiene los datos que hemos enviado y hace una solicitud `POST` a `/my-account/change-password`
![[Pasted image 20250812114816.png]]