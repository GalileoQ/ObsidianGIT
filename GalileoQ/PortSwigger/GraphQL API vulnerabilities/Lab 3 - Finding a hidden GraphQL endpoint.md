`TAREA:`Las funciones de gestión de usuarios de este laboratorio se basan en un endpoint oculto de GraphQL. No podrá encontrarlo simplemente haciendo clic en las páginas del sitio. El endpoint también cuenta con defensas contra la introspección.

Para resolver el laboratorio, busque el punto final oculto y elimínelo. `carlos`.

Obtenga más información sobre cómo trabajar con GraphQL en Burp Suite.

iniciamos sesión en el laboratorio y buscamos cualquier solicitud que parezca interesante para analizar
![[Pasted image 20250822112309.png]]

después de hacer una búsqueda en todos los endpoint no hemos conseguido nada interesante así que vamos a probar con los endpoint mas comunes en `GraphQL` de todas las solicitudes usare la `POST /login`
![[Pasted image 20250822112805.png]]

usando esta solicitud en el repeater y enviando el parámetro `PATCH` podemos ver que la respuesta nos indica que solo están permitidos los parámetros `GET , POST`
![[Pasted image 20250822113108.png]]

si cambiamos esta misma solicitud `POST` por una `GET` vemos que el servidor responde correctamente. así que vamos a ver que otros endpoint podemos consultar haciendo un llamado de solicitud `GET`
![[Pasted image 20250822113238.png]]

en este punto vamos a realizar un ataque de fuerza bruta para cada endpoint de los mas comunes para GraphQL que hemos conseguido haciendo una búsqueda rápida en internet
![[Pasted image 20250822113504.png]]

de todos estos comando particularmente el mas interesante de todos es `/api` ya que este tiene un estatus `400` diferente al de todos los demás. si analizamos la respuesta nos indica `Query not present` esto quiere decir que no esta presente la query necesaria para el endpoint `/api`
![[Pasted image 20250822114415.png]]

vamos hacer una búsqueda rápida en internet para intentar conseguir las query mas comunes y conseguimos esto:
![[Pasted image 20250822121113.png]]

probamos la query y efectivamente tenemos una respuesta `200 OK` 
![[Pasted image 20250822121149.png]]

si enviamos estos como una solicitud `Set introspection query` podemos ver que el servidor sigue respondiendo. en este caso nos indica que no soporta una query `instrospection` pero que la solicitud aun contiene el parámetro `__schema or __type`
![[Pasted image 20250822143428.png]]

ya que la consulta no permite la introspección. para pasar esto solo debemos agregar un carácter nuevo después de la consulta. Observe que la respuesta ahora incluye detalles completos de la introspección. Esto se debe a que el servidor está configurado para excluir las consultas que coinciden con la expresión regular. `"__schema{"`, con la que la consulta ya no coincide aunque sigue siendo una consulta de introspección válida.
![[Pasted image 20250822143821.png]]

ahora debemos hacer clic derecho en la solicitud y seleccione `GraphQL > Guardar consultas GraphQL en el mapa del sitio`
![[Pasted image 20250822144052.png]]

ahora en  `Site map` para ver las consultas de la API. vamos a la pestaña `GraphQL` y busque la consulta `getUser` y vamos a enviar esta solicitud al Repeater
![[Pasted image 20250822144510.png]]

ahora vamos a usar esta query para enumerar usuarios. nnormalmente el usuario con `id: 1 es Administator`
![[Pasted image 20250822150725.png]]

seguimos enumerando y encontramos al usuario carlos
![[Pasted image 20250822150634.png]]

copiamos la URL y la analizamos en la extensión `InQL` y encontramos una función llamada `deleteOrganizationUser` esta query será la que usaremos para intentar eliminar al usuario carlos
![[Pasted image 20250822150923.png]]


![[Pasted image 20250822151319.png]]


![[Pasted image 20250822151358.png]]