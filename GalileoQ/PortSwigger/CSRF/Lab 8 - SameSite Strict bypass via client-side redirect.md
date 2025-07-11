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

nuestra url es : `https://0a3500f3042d999182dfd8d400e000e3.web-security-academy.net/post/6` al final vamos a reemplazar `/post/6` por la ruta completa que nos indica el método `GET`

```python
#Inyeción 1

/post/comment/confirmation?postId=6
```

y efectivamente ocurre una redirección hacia la web donde hemos posteado nuestro comentario. y la web reestablece su url a la original
![[Pasted image 20250711171240.png]]

ahora bien. no quiero ser redirigido al `/post/6` por lo que realizaremos una nueva inyección

```python
#Inyeción 2

/post/comment/confirmation?postId=my-account/
```

![[Pasted image 20250711171736.png]]

después de la redirección nos da un error. eso es porque en la redirección que hacia a la web ya estaba plasmado esta ruta así que la vamos a modificar un poco

![[Pasted image 20250711174507.png]]

```python
#Inyeción 3

/post/comment/confirmation?postId=../my-account/
```

esto nos lleva al apartado de mi cuenta donde podemos actualizar el correo
![[Pasted image 20250711175000.png]]

luego copiamos nuestra solicitud `GET` a la cual le cambiamos el metodo y extraemos `change-email?email=gamuke%40poetswiggers.com&submit=1` para construir nuestra inyección numero 4 

```python
#Inyeción 4

/post/comment/confirmation?postId=../my-account/change-email?email=gamuke%40poetswiggers.com&submit=1
```

al enviar esto y esperar la redirección nos dice que falta el parámetro 'submit' esto se debe a que nuestra inyección contiene un `&` y la web no lo interpreta correctamente asi que lo tenemos que url encodear
![[Pasted image 20250711180206.png]]

asegúrate de cambiar el correo electrónico en este punto y cuando la web haga una redirección podrás ver si el cambio del correo electrónico se ha realizado efectivamente
```python
#Inyeción 5

/post/comment/confirmation?postId=../my-account/change-email?email=HACKER%40poetswiggers.com%26submit=1
```

efectivamente esto ha funcionado y hemos cambiado el correo electrónico haciendo un ataque CSRF haciendo omisión estricta de SameSite mediante redirección del lado del cliente
![[Pasted image 20250711180737.png]]

ahora vamos a construir la solicitud completa. para ellos debemos agregar las etiquitas `<script>` y también tenemos que agregar la función de JavaScript `document.location`

```python
#Inyeción 5

<script>
    document.location = "https://0a6500d704747bd48259f62700fb0078.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=HACKERPRO%40poetswiggers.com%26submit=1";
</script>

```

hemos cambiado el correo electrónico efectivamente haciendo omisión estricta de SameSite mediante redirección del lado del cliente 
![[Pasted image 20250711181942.png]]