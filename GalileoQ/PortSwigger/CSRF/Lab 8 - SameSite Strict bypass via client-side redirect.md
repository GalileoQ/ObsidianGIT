`TAREA:` La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Para solucionar este problema, realice un ataque CSRF que cambie la dirección de correo electrónico de la víctima. Debe usar el servidor de exploits proporcionado para alojar el ataque.

Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

iniciamos sesión en el laboratorio e interceptamos la petición. primero vamos a analizar la solicitud del login. y rápidamente podemos identificar:

`Set-Cookie: session=Ef7Myc0mxuMQ3n9u3Wn3RIqarpy4wArI` : esta es nuestra cookie de inicio de s
![[Pasted image 20250711163850.png]]