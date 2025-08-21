`TAREA:` Las funciones de gestión de usuarios de este laboratorio se basan en un endpoint GraphQL. El laboratorio presenta una vulnerabilidad de control de acceso que permite que la API revele los campos de credenciales del usuario.

Para resolver el laboratorio, inicie sesión como administrador y elimine el nombre de usuario. `carlos`.

Obtenga más información sobre cómo trabajar con GraphQL en Burp Suite.

para empezar vamos a intentar un inicio de sesión en el laboratorio y vamos a interceptar esta solicitud
![[Pasted image 20250821130744.png]]

podemos ver que el intento de inicio se sesión se envía como una mutación de `GraphQL` 
![[Pasted image 20250821130845.png]]

enviamos nuestra solicitud al repeater y luego la marcamos como `Set introspection query` para cambiar la solicitud y la enviamos para ver el resultado
![[Pasted image 20250821131020.png]]

ahora esta respuesta podemos copiarla para llevarla a [GraphQL Visualizer](http://nathanrandal.com/graphql-visualizer/) y de esta manera poder ver todo el flujo del grafo y entender mejor como funciona 
![[Pasted image 20250821131327.png]]

po
![[Pasted image 20250821131858.png]]