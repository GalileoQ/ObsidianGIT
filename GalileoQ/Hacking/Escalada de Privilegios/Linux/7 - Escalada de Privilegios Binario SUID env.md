Una vez dentro, tras hacer varias investigaciones, vemos que si buscamos binarios SUID vemos el de env, el cual es vulnerable:
![[Pasted image 20240612155938.png]]
Y ahora podemos lanzarnos una bash como root usando este binario de la siguiente forma:
![[Pasted image 20240612155941.png]]
