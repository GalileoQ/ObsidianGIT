Podemos lanzar este comando que sirve para ver qué archivos con extensión .sh son propiedad del usuario que somos (en este caso svc_acc); y vemos que hay uno que se llama ssh-alert.sh:
![[Pasted image 20240612160233.png]]
Y vemos que tenemos los permisos para ejecutar este script:
![[Pasted image 20240612160235.png]]
## Ver si tenemos permisos de append sobre un script
Aunque sí tenemos el permiso de añadir algo, un append:
![[Pasted image 20240612160239.png]]

Por tanto hacemos un append de esta manera, donde esto se añadirá al final del script:

![[Pasted image 20240612160244.png]]