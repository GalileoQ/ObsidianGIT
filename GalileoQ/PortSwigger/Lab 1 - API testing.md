
al registrarnos con las credenciales que nos dan notamos que podemos actualizar el correo. esto es un método que normalmente no esta securizado lo cual nos permite hacer solicitudes a diferentes endpoints de la api.

interceptamos la solicitud de actualización de contraseñas y podemos ver hacia a donde nos lleva el PATH de la ruta
![[Pasted image 20250616152933.png]]


### [Métodos de solicitudes en api almacenables en caché](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods#safe_idempotent_and_cacheable_request_methods)

La siguiente tabla enumera los métodos de solicitud HTTP y su categorización en términos de seguridad, capacidad de almacenamiento en caché e idempotencia.

|Método|Seguro|Idempotente|Almacenable en caché|
|---|---|---|---|
|[`GET`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/GET)|Sí|Sí|Sí|
|[`HEAD`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/HEAD)|Sí|Sí|Sí|
|[`OPTIONS`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/OPTIONS)|Sí|Sí|No|
|[`TRACE`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/TRACE)|Sí|Sí|No|
|[`PUT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/PUT)|No|Sí|No|
|[`DELETE`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/DELETE)|No|Sí|No|
|[`POST`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST)|No|No|Condicional*|
|[`PATCH`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/PATCH)|No|No|Condicional*|
|[`CONNECT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/CONNECT)|No|No|No|

La tabla de la documentación nos muestra que PATH normalmente no es un método seguro lo cual nos permite hacer diferentes solicitudes a los distintos endpoints de la api. en este caso cambiando el método PATH por el método GET.

enumerando las diferentes rutas de la api encontramos la documentación de la api. la cual nos indica que existen 3 métodos habilitados. en este caso son: `GET, DELETE, PATH` y tambien nos dice que la ruta en la cual reciden los usuarios es `/api/user/NombreDeUsuario`
![[Pasted image 20250616152320.png]]

llamamos al método `DELETE` y proporcionamos la ruta donde reside el usuario Carlos. y de esta manera podemos eliminar al usuario Carlos 
![[Pasted image 20250616154436.png]]