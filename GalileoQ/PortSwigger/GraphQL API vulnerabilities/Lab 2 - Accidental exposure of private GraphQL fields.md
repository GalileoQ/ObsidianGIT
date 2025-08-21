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

podemos ver que las query `getBlogPost(id:int!): BlogPost` y `getAllBlogPost: [BlogPost!]!` apuntan al titulo de la solicitud y al autor de la solicitud sin embargo la query `getUser(id:Int!): User`  parece ser la encargada de la identificación del usuario y la contraseña
![[Pasted image 20250821131858.png]]

ahora si vamos a la solicitud principal y vemos como se envía el nombre y la contraseña desde la pestaña de `GraphQl` podemos eliminar la contraseña y probar con el nombre de administrator y obtenemos una respuesta de estado `200 OK` 
![[Pasted image 20250821132542.png]]

así que vamos a regresar a la solicitud de `Set introspection query` para después enviarla a `Save Graph queries to side map`
![[Pasted image 20250821132821.png]]

en esta parte la que nos interesa analizar es la query que hemos visto en el grafico que es la encargada de la información del usuario `getUser(id:Int!): User` la enviamos al repeater para analizarla
![[Pasted image 20250821133144.png]]

si enviamos la solicitud no vemos nada pero si analizamos el `GraphQl` y cambiamos el `id:0` `id:1` podemos ver las credenciales del usuario administrador. y no solo eso. podríamos jugar con este numero tanto como sea posible ya que el identificador no tiene restricciones así que podríamos enumerar tantos usuarios como existan 
![[Pasted image 20250821133409.png]]

credenciales del usuario con el `id:2` 
![[Pasted image 20250821133628.png]]

credenciales del usuai