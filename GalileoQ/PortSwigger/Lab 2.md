Para resolver el laboratorio, aprovecha un endpoint API oculto para comprar una **chaqueta de cuero ligera l33t** . Puedes iniciar sesión en tu cuenta con las siguientes credenciales: `wiener:peter`.

lo primero es identificar los endpoints hacia donde apunta la api para poder identificar la documentación y el comportamiento
![[Pasted image 20250616164630.png]]

revisamos todas las rutas `/api/products/price` pero no encontramos nada. en este caso simplemente cambiando el método a `POST` podemos ver una respuesta por parte del servidor que nos da mas información sobre el funcionamiento de la api tamnien podemos identificar que el `Content-Type` es `aplication/json;` 

![[Pasted image 20250616164758.png]]

`PATCH`
revisamos el PATCH y nos indican que solo `Content-Type application/json` es permitido así que debemos ingresar este parámetro y seguir investigando
![[Pasted image 20250616170431.png]]

al ingresar el `Content-Type: application/json` debemos pasar un parámetro `{}` vacío ya que no sabemos realmente cual es el parámetro de esa solicitud. después de enviar la solicitud nos indica que el parámetro `price` falta en la solicitud. de esta manera identificamos que `price` es el parámetro del json
![[Pasted image 20250616170743.png]]

agregamos el `Content-Type: application/json` pasando el parámetro price con un valor de 10 y obtenemos una respuesta de estado `200 OK` 
![[Pasted image 20250616171150.png]]

esto no modifica el precio del producto que tenemos en la tienda pero si lo modifica en el apartado `HOME` asi que solo debemos modificar el `Content-Type: application/json` de nuevo 
![[Pasted image 20250616171341.png]]