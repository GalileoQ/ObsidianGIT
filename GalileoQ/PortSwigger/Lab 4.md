Para resolver el laboratorio, encuentra y explota una vulnerabilidad de asignación masiva para comprar una **chaqueta de cuero ligera l33t** . Puedes iniciar sesión en tu cuenta con las siguientes credenciales: `wiener:peter`.

vamos a iniciar sesión con nuestras credenciales. vamos a agregar la chaqueta a nuestro carrito y vamos a verificar los puntos finales de la api. en este caso vemos un `GET` y un  `POST` sobre `/api/chekout` así que vamos a enviar estas dios solicitudes al repiter para poder analizarlas
![[Pasted image 20250620151143.png]]

la solicitud `GET` nos muestra una información sobre un descuento `chosen_discount`
![[Pasted image 20250620152032.png]]

esta información es importante ya que podemos incrustarla en la solicitud `POST` y enviar la solicitud. lo que nos permite realizar la orden con un descuento del `100%` y podemos ver 
![[Pasted image 20250620152217.png]]