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










clic derecho en la solicitud y seleccione Herramientas de interacción ó (Engagement tools) después  > Generarte CSRF PoC. 
![[Pasted image 20250703135954.png]]

podemos ver que en el CSRF Poc se encuentra el valor del token csrf. por lo que la estructura de esta Request es diferente. 
![[Pasted image 20250703140326.png]]

