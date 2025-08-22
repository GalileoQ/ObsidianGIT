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

ya que la consulta no permite la introspección. para pasar esto solo debemos agregar un carácter nuevo después de la consulta
![[Pasted image 20250822143821.png]]

