`TAREA:`
La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Para solucionar este problema, realice un ataque CSRF que cambie la dirección de correo electrónico de la víctima. Debe usar el servidor de exploits proporcionado para alojar el ataque.

Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

iniciamos sesión en el laboratorio con las credenciales proporcionadas
![[Pasted image 20250711151627.png]]

si analizamos la solicitud `POST` encontramos que tenemos una `cookie` para el inicio de sesión y también vemos que tiene una fecha de vencimiento configurada. además de eso esta configurada para `HttpOnly` es una cookie y significa que la cookie es segura y solo puede ser accedida por el protocolo HTTP, no por scripts del lado del cliente como JavaScript. Esto ayuda a prevenir ataques de secuencias de comandos entre sitios (XSS). 
![[Pasted image 20250711152650.png]]

ahora interceptamos la solicitud de cambio de correo y la enviamos al `Repeater`
![[Pasted image 20250711151709.png]]

enviamos esta solicitud y seguimos la redirección. no obtenemos la `cookie` `secure: HttpOnly` así que vamos a cambiar el método
![[Pasted image 20250711152212.png]]

cambiamos el método `POST` por un `GET` y podemos ver que efectivamente tenemos la `cookie` `secure; HttpOnly`
![[Pasted image 20250711155228.png]]

ahora si intentamos cambiar el método a la solicitud de cambio de correo la respuesta del lado del servidor es `"Method Not Allowed"`
![[Pasted image 20250711155814.png]]

investigando en internet encontramos una suplantación de método: ya que los formularios HTML no soportan `PUT, PATCH, DELETE` existen otros marcos que a través de parámetros ocultos sobrescribir el comportamiento del método

en este caso podríamos usar una solicitud `GET` pero le estamos indicando a la web que con el parámetro oculto `_method` que lo interprete como una solicitud `POST`

```python
<form action="/foo/bar" method="POST">
    <input type="hidden" name="_method" value="PUT">
    <input type="hidden" name="_token" value="<?php echo csrf_token(); ?>">
</form>
```

![[Pasted image 20250711160109.png]]

vamos a crear nuestro CSRF PoC. 
![[Pasted image 20250711155352.png]]

primero vamos a cambiar el método principal de la solicitud de `POST` a `GET` y luego vamos a inyectar nuestro parámetro siguiendo las reglas de la suplantación de parámetros 

```python
<input type="hidden" name="_method" value="PUT">
```

y en este caso el parámetro que nos interesa es una solicitud `POST` y nos quedaría nuestra solicitud de la siguiente forma:

```python

```

![[Pasted image 20250711161526.png]]