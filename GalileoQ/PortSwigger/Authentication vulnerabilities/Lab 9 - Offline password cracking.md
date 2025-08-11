`TAREA:` Este laboratorio almacena el hash de la contraseña del usuario en una cookie. También contiene una vulnerabilidad XSS en la función de comentarios. Para resolver el laboratorio, obtenga la información de Carlos. `stay-logged-in`cookie y usarla para descifrar su contraseña. Luego, inicia sesión como `carlos`y eliminar su cuenta desde la página "Mi cuenta".

- Sus credenciales: `wiener:peter`
- Nombre de usuario de la víctima: `carlos`

iniciamos sesión como siempre con las credenciales que nos da el laboratorio y vamos a capturar la solicitud para analizarla
![[Pasted image 20250811124154.png]]

ahora hemos activado el parámetro `stay-logged-in` pero no tenemos un cookie o un has MD5 así que tenemos que buscar otra opción 
![[Pasted image 20250811124306.png]]

ahora. tenemos un apartado de blogs donde podemos realizar comentarios. así que vamos a realizar pruebas aquí. 
![[Pasted image 20250811124701.png]]

de esta forma hemos generado un ataque XSS lo cual nos da una pequeña pista de lo que podemos hacer ahora.
![[Pasted image 20250811124816.png]]

ahora lo que vamos a intentar es capturar la 
![[Pasted image 20250811125041.png]]

enviamos el comentario y vamos a revisar nuestro servidor de exploit. donde vemos que efectivamente hemos conseguido una cookie
![[Pasted image 20250811125319.png]]

decodeamos la cookie y obtenemos las credenciales del usuario carlos
![[Pasted image 20250811125414.png]]

ahora vamos a decodear el HASH MD5 del usuario carlos y obtenemos la contraseña en texto plano
![[Pasted image 20250811125537.png]]

ahora podemos completar la terea que nos solicitan que seria eliminar la cuenta del usuario carlos
![[Pasted image 20250811125707.png]]

y de esta manera podemos resolver el laboratorio
![[Pasted image 20250811125738.png]]