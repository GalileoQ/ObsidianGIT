Este laboratorio tiene una función de verificación de existencias que obtiene datos de un sistema interno.

Para resolver el laboratorio, utilice la función de verificación de existencias para escanear el interior `192.168.0.X`rango para una interfaz de administrador en el puerto `8080`, luego úsalo para eliminar al usuario `carlos`.

vamos al laboratorio y podemos hacer un `Check stock` para ver esta solicitud
![[Pasted image 20250818104450.png]]

podemos ver que el parámetro `StockApi` parece estar resolviendo a una URL asi que vamos a enviar esto al intruder para intentar hacer algún tipo de ataque
![[Pasted image 20250818104606.png]]

ya sabemos que el laboratorio se resuelve haciendo un escaneo para encontrar otro endpoint en el servidor. así que debemos usar la URL que nos da el enunciado para resolver esto. `http://192.168.0.1:8080/admin` agregamos 8080 ya que este es nuestro proxy de burpsuite y `/admin` porque queremos intentar conseguir un panel admin en el otro endpoint ya que es lo mas normal. 
![[Pasted image 20250818104801.png]]

después de seleccionar la lista de payload en `numbers` y decirle que haga un escaneo desde la IP 1 hasta 255 realizamos el ataque y solo tenemos una sola solicitud de estatus `200` así que vamos a ver esta 
![[Pasted image 20250818105101.png]]

ahora recordemos que nuestra tarea es eliminar al usuario carlos y esto lo podemos intentar mediente la solicitud a `http://192.168.0.196:8080/admin/delete?username=carlos`
![[Pasted image 20250818105342.png]]

