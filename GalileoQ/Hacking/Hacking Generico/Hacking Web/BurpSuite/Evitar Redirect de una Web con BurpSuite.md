Para impedir un redirect, podemos usar burpsuite:
![[Pasted image 20230327072152.png]]
Entonces lo que vamos a hacer es indicar de interceptar la respuesta con burpsuite, y de esta forma podremos ver y manipular ese redirect:
![[Pasted image 20230327072342.png]]
Hacemos clic en forward y nos encontramos con el redirect, el cual podemos manipular y poner /admin:
![[Pasted image 20230327072436.png]]
