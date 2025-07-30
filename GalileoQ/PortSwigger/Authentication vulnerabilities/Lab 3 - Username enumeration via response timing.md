Este laboratorio es vulnerable a la enumeración de nombres de usuario mediante sus tiempos de respuesta. Para solucionarlo, enumere un nombre de usuario válido, utilice un método de fuerza bruta para obtener su contraseña y acceda a su página de cuenta.

- Sus credenciales: `wiener:peter`
- [Nombres de usuario de los candidatos](https://portswigger.net/web-security/authentication/auth-lab-usernames)
- [Contraseñas de candidatos](https://portswigger.net/web-security/authentication/auth-lab-passwords)

primero iniciamos sesión y capturamos la solicitud para poder analizarla
![[Pasted image 20250730151404.png]]

en la solicitud de login por ahora no vemos nada raro así que vamos a analizar una solicitud fallida y así poder compararlas.
![[Pasted image 20250730152025.png]]

principalmente vemos que el `Length` es diferente y también podemos extraer el mensaje de `Invalid username or password`
![[Pasted image 20250730152552.png]]

haciendo pruebas podemos observar que tenemos un mensaje nuevo. este mensaje nos indica que hemos realizado demasiados intentos incorrectos y que podemos intentarlo de nuevo en 30 minutos. esto es debido a que el servidor identifica demasiadas solicitudes provenientes desde nuestra dirección IP y nos pone en una lista negra
![[Pasted image 20250730152738.png]]

ya sabemos que manipulando la cabecera de la solicitud podemos implementar el parámetro `X-Forwarded-For` para manipular nuestra dirección IP de cara al servidor. de esta forma tenemos la web habilitada nuevamente para seguir haciendo pruebas.
![[Pasted image 20250730153415.png]]

enviamos la solicitud al intruder y seleccionamos el ultimo valor de la IP ya que nos interesa hacer una iteración sobre este valor para tener una dirección IP única para cada nombre de usuario. también seleccionaremos el nombre de usuario ya que este debe iterar por todos los nombres de nuestra lista de usuarios. y específicamente debemos seleccionar el tipo de ataque `Pitchfork attack` ya que este es el que nos ayudara en cuento al manejo de la IP y el nombre. de lo contrario el ataque seria con una misma dirección IP y el servidor la bloquearía de nuevo
![[Pasted image 20250730154322.png]]

