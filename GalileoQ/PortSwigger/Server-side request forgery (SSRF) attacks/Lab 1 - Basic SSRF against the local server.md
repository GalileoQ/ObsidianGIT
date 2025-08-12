`TAREA:`Este laboratorio tiene una función de verificación de existencias que obtiene datos de un sistema interno.

Para resolver el laboratorio, cambie la URL de verificación de existencias para acceder a la interfaz de administración en `http://localhost/admin`y eliminar el usuario `carlos`.

iniciamos sesión y vamos a revisar un producto para luego darle `Check stock`
![[Pasted image 20250812154436.png]]

capturamos la solicitud de `Check stock` y al analizarla vemos que el parámetro `stockApi` hace referencia a una url. vamos a cambiar esta url para intentar hacer un redireccionamiento
![[Pasted image 20250812154902.png]]

cambiamos la url y vemos que el servidor nos da un estatus `200 OK` así que vamos a ver esta solicitud en el navegador
![[Pasted image 20250812155205.png]]

ahora podemos eliminar al usuario carlos. pero el problema es que no estamos logeados así que no nos deja.
![[Pasted image 20250812155147.png]]

inspeccionamos la web para identificar que la `URL` para eliminar al usuario carlos es 
![[Pasted image 20250812155852.png]]