`TAREA:` La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Para solucionar este problema, realice un ataque CSRF que cambie la dirección de correo electrónico de la víctima. Debe usar el servidor de exploits proporcionado para alojar el ataque.

Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

iniciamos sesión en el laboratorio e interceptamos la petición. primero vamos a analizar la solicitud del login. y rápidamente podemos identificar:

`Set-Cookie: session=Ef7Myc0mxuMQ3n9u3Wn3RIqarpy4wArI` : esta es nuestra cookie de inicio de sesión

`Secure; HttpOnly` este atributo nos indica que esta habilitado solo para solicitudes https por lo que no es accesible desde JavaScript

`SameSite=Strict` esto significa que esta cookie de sesión no se enviara ni se usara en ninguna solicitud entre los diferentes sitios de la web
![[Pasted image 20250711163850.png]]

realizamos un cambio de correo y analizamos la petición. en este caso vemos que  no existe un token que invalide un ataque CSRF y tampoco vemos una csrfKey para la cookie
![[Pasted image 20250711164703.png]]

al enviar esta solicitud al `Repeater` para cambiar el método obtenemos un código de estado `200 OK` y esto es muy importante porque si podemos encontrar una redirección que valide el servidor potencialmente podemos realizar una solicitud `GET` con una redirección hacia un cambio de correo 
![[Pasted image 20250711165010.png]]

buscamos un post y escribimos un comentario llenando todos los parámetros para ver como se comporta y que hace esta solicitud 
![[Pasted image 20250711165644.png]]

la respuesta nos dice: `You will be redirected momentarily` esto es super importante ya que la web nos indica que vamos a ser redireccionados por lo que esta solicitud podría funcionar 
![[Pasted image 20250711165730.png]]

analizamos la solicitud con el burpsuite y podemos identificar una redirección en la función `('/post')` que esta siendo llamada desde la ruta  externa del JavaScript
![[Pasted image 20250711170141.png]]

ahora analizamos la solicitud `GET` hacia el archivo `js` podemos ver que la función de redirección hace un llamado a (blogPath) y sabemos que esto es igual a `/post` y también vemos una espera de `3000 ms` 3 segundos y recupera el `"postid"` este parámetro es muy importante porque justamente este podría llegar a ser controlado por nosotros 
![[Pasted image 20250711170404.png]]

en la solicitud de nuestro post podemos ver que la solicitud `GET` llama a la ruta `/post/comment/confirmation?postId=6` así que esto podría llevarnos a la ruta de redirección de la web
![[Pasted image 20250711171054.png]]


![[Pasted image 20250711171240.png]]