Para resolver el laboratorio, aprovecha un endpoint API oculto para comprar una **chaqueta de cuero ligera l33t** . Puedes iniciar sesión en tu cuenta con las siguientes credenciales: `wiener:peter`.

lo primero es identificar los endpoints hacia donde apunta la api para poder identificar la documentación y el comportamiento
![[Pasted image 20250616164630.png]]

revisamos todas las rutas `/api/products/price` pero no encontramos nada. en este caso simplemente cambiando el método a `POST` podemos ver una respuesta por parte del servidor que nos da mas información sobre el funcionamiento de la api tamnien podemos identificar que el `Content-Type` es `aplication/json;` 

![[Pasted image 20250616164758.png]]

