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

esto es importante saberlo ya que si esta cookie de sesión no tiene ninguna prevención contra fuerza bruta podemos realizar un ataque pera buscar la contraseña. vamos a capturar la solicitud `GET` de la cuenta del usuario `wiener` para poder enviarla al intruder y hacer nuestras pruebas
![[Pasted image 20250811120250.png]]

ahora lo que nos interesa es intentar conseguir una cookie que contenga el nombre del usuario y la contraseña en formato MD5 ya que sabemos que este es el formato en el que se construye la solicitud. asi que vamos a seleccionar la cookie para realizar el ataque. es importante que en el segundo apartado `Payload `
![[Pasted image 20250811120633.png]]