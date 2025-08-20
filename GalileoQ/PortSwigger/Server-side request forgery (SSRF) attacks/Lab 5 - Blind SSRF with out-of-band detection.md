`TAREA:` Este sitio utiliza un software de análisis que obtiene la URL especificada en el encabezado Referer cuando se carga una página de producto.

Para resolver el laboratorio, utilice esta funcionalidad para generar una solicitud HTTP al servidor público Burp Collaborator.

seleccionamos cualquier producto para analizar la solicitud
![[Pasted image 20250820113217.png]]


![[Pasted image 20250820113155.png]]

enviamos la solicitud al Repeater y seleccionamos "Insert Collaborator Payload" para reemplazar el dominio original con un dominio generado por Burp Collaborator. podemos ver que en la parte de abajo se nos ha generado una nueva dirección de dominio que vamos a reemplazar en el parámetro `Referer` 
![[Pasted image 20250820113957.png]]

hacemos clic en `Poll now` y vemos la solicitud DNS que se ha hecho desde y hacia nuestro nuevo dominio
![[Pasted image 20250820114407.png]]

de esta forma podemos ver la solicitud en la web y hemos resuelto el laboratorio
![[Pasted image 20250820114435.png]]