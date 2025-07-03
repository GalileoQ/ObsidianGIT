Tarea: La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Intenta bloquear ataques CSRF, pero solo aplica defensas a ciertos tipos de solicitudes.
Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.
Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales:`wiener:peter`

iniciamos el laboratorio con las credenciales. vamos a interceptar la solicitud de `Update email` para analizar su funcionamiento
![[Pasted image 20250703135512.png]]

podemos ver la solicitud e identificar que esta tiene un token CSRF. este token 
![[Pasted image 20250703135653.png]]

### Defensas comunes contra CSRF

Hoy en día, detectar y explotar vulnerabilidades CSRF con éxito suele implicar eludir las medidas anti-CSRF implementadas por el sitio web objetivo, el navegador de la víctima o ambos. Las defensas más comunes que encontrará son las siguientes:

- **Tokens CSRF** : Un token CSRF es un valor único, secreto e impredecible generado por la aplicación del servidor y compartido con el cliente. Al intentar realizar una acción confidencial, como enviar un formulario, el cliente debe incluir el token CSRF correcto en la solicitud. Esto dificulta enormemente que un atacante cree una solicitud válida en nombre de la víctima.
    
- **Cookies de SameSite** : SameSite es un mecanismo de seguridad del navegador que determina cuándo se incluyen las cookies de un sitio web en las solicitudes procedentes de otros sitios web. Dado que las solicitudes para realizar acciones sensibles suelen requerir una cookie de sesión autenticada, las restricciones adecuadas de SameSite pueden impedir que un atacante las ejecute entre sitios. Desde 2021, Chrome aplica `Lax`las restricciones de SameSite por defecto. Dado que este es el estándar propuesto, prevemos que otros navegadores importantes adoptarán este comportamiento en el futuro.
    
- **Validación basada en referencias** : algunas aplicaciones utilizan el encabezado HTTP Referer para intentar defenderse de ataques CSRF, normalmente verificando que la solicitud se originó en el dominio de la aplicación. Esto suele ser menos efectivo que la validación de tokens CSRF.

capturamos la solicitud y la enviamos al repeater. podemos ver que la respuesta es 302 Found. en este punto cambiamos el método de la solicitud `Change request method` 
![[Pasted image 20250703151340.png]]

si cambiamos el metodo de la solicitud podemos observar que la respuesta sigue siendo `HTTP/2 302 Found` por lo que podemos trabajar con esta request.
![[Pasted image 20250703151630.png]]

clic derecho en la solicitud y seleccione Herramientas de interacción ó (Engagement tools) después  > Generarte CSRF PoC. modificamos el correo electrónico y copiamos el `CSRF HTML`
![[Pasted image 20250703151826.png]]

pegamos la request CSRF en nuestro servidor de exploit y `Deliver exploit to victim` y de esta manera podemos resolver el lab
![[Pasted image 20250703152114.png]]

### OPCIÓN 2

después de capturara la solicitud y enviarla al repeater podemos observar que la respuesta es un `HTTP/2 302 Found` 
![[Pasted image 20250703153113.png]]

si eliminamos el token observamos que la respuesta es `HTTP/2 400 Bad Request` y nos indica que el falta el parámetro `csrf` es importante ver y analizar como se comporta la request o solicitud y la response o respuesta. porque de esta manera podemos obtener información de como se esta comportando el servidor
![[Pasted image 20250703153325.png]]

cambiamos nuevamente el método de solicitud `Change request method` 
![[Pasted image 20250703153618.png]]

esto nos transforma la solicitud `POST` en una `GET` pero si prestamos atención a la respuesta sigue siendo `HTTP/2 302 Found` 
![[Pasted image 20250703153717.png]]

si eliminamos el token en esta parte de la solicitud podemos observar que nuestra respuesta sigue siendo `HTTP/2 302 Found` importante tener esto en cuenta Algunas aplicaciones validan correctamente el token cuando está presente, pero omiten la validación si se omite el token.

En esta situación el atacante puede eliminar todo el parámetro que contiene el token (no solo su valor) para eludir la validación y lanzar un ataque CSRF:

```python
POST /email/change HTTP/1.1 
Host: vulnerable-website.com 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 25 
Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm 
email=pwned@evil-user.net
```

![[Pasted image 20250703153921.png]]

ahora reconstruimos la solicitud y debería quedarnos algo así:

```python
<form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email"> 
	<input type="hidden" name="email" value="anything%40web-security-academy.net"> 
</form> 
<script> 
	document.forms[0].submit(); 
</script>
```

en esta caso debemos tener precaución con los comandos que están urlencodeados ya que si miramos los log podemos darnos cuenta que el comando `%40` no lo esta procesando correctamente esto en `ASCII` representa el `@` asi que podemos reemplazarlo y de esta manera logramos modificar el correo electrónico de la victima  
![[Pasted image 20250703154708.png]]

