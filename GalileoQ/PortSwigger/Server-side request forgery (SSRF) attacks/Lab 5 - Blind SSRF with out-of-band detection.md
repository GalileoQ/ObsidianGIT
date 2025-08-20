`TAREA:` Este sitio utiliza un software de análisis que obtiene la URL especificada en el encabezado Referer cuando se carga una página de producto.

Para resolver el laboratorio, utilice esta funcionalidad para generar una solicitud HTTP al servidor público Burp Collaborator.

seleccionamos cualquier producto para analizar la solicitud
![[Pasted image 20250820113217.png]]


![[Pasted image 20250820113155.png]]

enviamos la solicitud al Repeater y seleccionamos "Insert Collaborator Payload" para reemplazar el dominio original con un dominio generado por Burp Collaborator. 
![[Pasted image 20250820113957.png]]