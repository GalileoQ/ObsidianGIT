`TAREA:`
Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.

Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

como siempre iniciamos sesión en el lab con las credenciales que nos proporcionan e interceptamos la solicitud para luego enviarla al repeater
![[Pasted image 20250710162852.png]]

después de interceptar la solicitud la enviamos al repeater y podemos tirar (Drop) la intercepción para no realizar ningún cambio 

podemos ver que tenemos tanto la `cookie` de inicio de sesión como el `csrf`
![[Pasted image 20250710163229.png]]

ahora realizamos una búsqueda y analizamos la solicitud para identificar sus parámetros
![[Pasted image 20250710163311.png]]

una ves tenemos la solicitud la enviamos al repeater para analizar la respuesta del lado del servidor. en este caso tenemos una respuesta `200 OK` y nos devuelve el parámetro `Set-cookie: LastSearchTerm` esta cookie contiene el parámetro de nuestra ultima búsqueda es decir `test` así que vamos a validar esto y si no cumple con la validación correcta podemos explotar esta vulnerabilidad y generar una cabecera dinámica que contenga el parámetro `Set-cookie`
![[Pasted image 20250710163437.png]]

realizamos la validación correspondiente y efectivamente obtenemos una respuesta `200 OK` donde el parámetro `Set-Cookie` contiene el valor `c` 
![[Pasted image 20250710164155.png]]