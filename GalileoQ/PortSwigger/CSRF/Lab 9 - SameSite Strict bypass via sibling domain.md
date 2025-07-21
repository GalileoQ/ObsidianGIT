El chat en vivo de este laboratorio es vulnerable al secuestro de WebSockets entre sitios (CSWSH). Para solucionar el problema, inicie sesión en la cuenta de la víctima.

Para ello, utilice el servidor de exploits proporcionado para ejecutar un ataque CSWSH que exfiltre el historial de chat de la víctima al servidor predeterminado de Burp Collaborator. El historial de chat contiene las credenciales de inicio de sesión en texto plano.


iniciamos el chat y de inmediato podemos recopilar información. sabemos que se llama `Hal Pline` y que su estatus es `CONNECTED`
![[Pasted image 20250721132918.png]]

analizando la solicitud vemos el encabezado `X-Frame-Options: SAMEORIGIN` indica que la página web solo puede ser mostrada en un frame si el sitio que intenta incrustarla está en la misma origen (dominio, esquema y puerto) que la página original
![[Pasted image 20250721133525.png]]

analizando todos los endpoints de la web nos damos cuenta que todos cuenta con la etiqueta `X-Frame-Options: SAMEORIGIN` pero al analizar la solicitud de un post encontramos un endpoints diferente. 
![[Pasted image 20250721133911.png]]

`https://cms-0a9500be04bf9ea5811070e6005900ee.web-security-academy.net` 
este endpo