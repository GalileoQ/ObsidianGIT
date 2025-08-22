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

