`TAREA:` La página del blog de este laboratorio contiene una entrada oculta con una contraseña secreta. Para resolver el laboratorio, encuentra la entrada oculta e introduce la contraseña.


![[Pasted image 20250820125828.png]]

en la respuesta observamos que el parámetro `getAllBlogPosts` contiene todos los post pero falta el numero `3`
![[Pasted image 20250820144317.png]]


![[Pasted image 20250820125802.png]]

enviamos la solicitud y vemos que la respuesta tiene un parámetro `postPassword` pero parece estar oculto.
![[Pasted image 20250820151254.png]]

vamos a verificar esto en la sección de `InQL` usando la URL de la solicitud `POST /graphql/v1` y enviamos esta solicitud a analizar. efectivamente encontramos el parámetro `postPassword`

`NOTA:` esta sección se ha realizado con la extensión `InQL` que puedes descargar desde el apartado de extensiones 
![[Pasted image 20250820151342.png]]

vamos a copiar todos los parámetros que hemos encontrado y lo vamos a enviar al repeater para reemplazarlos en la sección de `InQL` despues de enviar la solicitud observamos que la respuesta de los post ahora tienen mucha mas información como `autor, date` 
![[Pasted image 20250820151848.png]]