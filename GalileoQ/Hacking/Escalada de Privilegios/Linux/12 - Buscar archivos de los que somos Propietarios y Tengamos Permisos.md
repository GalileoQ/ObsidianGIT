Podemos lanzar este comando que sirve para ver qué archivos con extensión .sh son propiedad del usuario que somos (en este caso svc_acc); y vemos que hay uno que se llama ssh-alert.sh:
![[Untitled 1 3.png]]
Y vemos que tenemos los permisos para ejecutar este script:
![[Untitled 2 3.png]]
## Ver si tenemos permisos de append sobre un script
Aunque sí tenemos el permiso de añadir algo, un append:
![[Untitled 3 1.png]]
Por tanto hacemos un append de esta manera, donde esto se añadirá al final del script:
![[Untitled 4 1.png]]
