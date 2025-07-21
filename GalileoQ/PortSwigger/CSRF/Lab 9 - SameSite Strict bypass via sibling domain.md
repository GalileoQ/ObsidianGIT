El chat en vivo de este laboratorio es vulnerable al secuestro de WebSockets entre sitios (CSWSH). Para solucionar el problema, inicie sesión en la cuenta de la víctima.

Para ello, utilice el servidor de exploits proporcionado para ejecutar un ataque CSWSH que exfiltre el historial de chat de la víctima al servidor predeterminado de Burp Collaborator. El historial de chat contiene las credenciales de inicio de sesión en texto plano.


iniciamos el chat y de inmediato podemos recopilar información. sabemos que se llama `Hal Pline` y que su estatus es `CONNECTED`
![[Pasted image 20250721132918.png]]

analizando la solicitud vemos el encabezado `X-Frame-Options: SAMEORIGIN` indica que la página web solo puede ser mostrada en un frame si el sitio que intenta incrustarla está en la misma origen (dominio, esquema y puerto) que la página original
![[Pasted image 20250721133525.png]]

analizando todos los endpoints de la web nos damos cuenta que todos cuenta con la etiqueta `X-Frame-Options: SAMEORIGIN` pero al analizar la solicitud de un post encontramos un endpoints diferente. 
![[Pasted image 20250721133911.png]]

`https://cms-0a9500be04bf9ea5811070e6005900ee.web-security-academy.net` 
este endpoint nos lleva a una web de login donde podemos hacer una comprobación rápida para vulnerabilidades CSRF y logramos identificarla de la siguiente manera.
![[Pasted image 20250721134247.png]]

en el WebSockets History podemos ver el mensaje que hacemos al servidor y su contenido
![[Pasted image 20250721134538.png]]

y también el mensaje que regresa el servido
![[Pasted image 20250721134733.png]]

podemos enviar esta petición al repeater para ver como la respuesta y todos los mensajes que hemos intercambiado con el WebSocket
![[Pasted image 20250721162752.png]]

ahora bien. podemos ver que la dirección del websocket esta definida en la cabecera de respuesta del mensaje lo cual será útil para luego hacer nuestro ataque CSRF
![[Pasted image 20250721163118.png]]

por ultimo encontramos la funcion del chat 
![[Pasted image 20250721164204.png]]