`TAREA:`
La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Para solucionar este problema, realice un ataque CSRF que cambie la dirección de correo electrónico de la víctima. Debe usar el servidor de exploits proporcionado para alojar el ataque.

Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

iniciamos sesión en el laboratorio con las credenciales proporcionadas
![[Pasted image 20250711151627.png]]

si analizamos la solicitud `POST` encontramos que tenemos una `cookie` para el inicio de sesión y temabien vemos que tiene 
![[Pasted image 20250711152650.png]]

interceptamos la solicitud y la enviamos al `Repeater`
![[Pasted image 20250711151709.png]]

enviamos esta solicitud y seguimos la redirección. 
![[Pasted image 20250711152212.png]]