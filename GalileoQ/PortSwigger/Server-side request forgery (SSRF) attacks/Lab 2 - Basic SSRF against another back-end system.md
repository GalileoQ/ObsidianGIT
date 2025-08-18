Este laboratorio tiene una función de verificación de existencias que obtiene datos de un sistema interno.

Para resolver el laboratorio, utilice la función de verificación de existencias para escanear el interior `192.168.0.X`rango para una interfaz de administrador en el puerto `8080`, luego úsalo para eliminar al usuario `carlos`.

vamos al laboratorio y podemos hacer un `Check stock` para ver esta solicitud
![[Pasted image 20250818104450.png]]

podemos ver que el parámetro `StockApi` parece estar resolviendo a una URL asi que vamos a enviar esto al intruder para intentar hacer algún tipo de ataque
![[Pasted image 20250818104606.png]]

ya sabemos que el laboratorio se resuelve haciendo un escaneo para encontrar otro endpoint en el servidor. asi que debemos usar 
![[Pasted image 20250818104801.png]]