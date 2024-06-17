Si ejecutamos el comando sudo -l podemos ver que el usuario www-data puede ejecutar cualquier comando que se encuentre dentro del directorio html o también creado con vim:
![[Pasted image 20240612160449.png]]

Ahora con vim podremos crear un código que nos mande una bash como root y lo podemos editar como sudo porque tenemos ese privilegio:
![[Pasted image 20240612160505.png]]

Pues ahora una vez abierto el fichero, en la parte de abajo de vim nosotros podemos establecer instrucciones, que por ejemplo para elevar privilegios escribiremos esto ya que somos usuario sudo:
![[Pasted image 20240612160508.png]]

Y ahora a continuación si damos a enter se habrá guardado lo de antes; y si luego hacemos clic en escape y volvemos a hacer shift y dos puntos para escribir shell, ya nos lanzará una bash de root, por tanto damos a enter y lo tenemos:
![[Pasted image 20240612160533.png]]

-------------------------------------------------------------------

## OTRO EJEMPLO

Ejecutamos el comando sudo -l para ver que acciones podemos ejecutar con el usuario que somos; por ejemplo en este caso podemos ejecutar vim:
![[Pasted image 20240612160547.png]]

Ahora con vim podremos crear un código que nos mande una bash como root y lo podemos editar como sudo porque tenemos ese privilegio:
![[Pasted image 20240612160552.png]]

Pues ahora una vez abierto el fichero, en la parte de abajo de vim nosotros podemos establecer instrucciones, que por ejemplo para elevar privilegios escribiremos esto ya que somos usuario sudo:
![[Pasted image 20240612160556.png]]

Y ahora a continuación si damos a enter se habrá guardado lo de antes; y si luego hacemos clic en escape y volvemos a hacer shift y dos puntos para escribir shell, ya nos lanzará una bash de root, por tanto damos a enter y lo tenemos:
![[Pasted image 20240612160600.png]]

![[Pasted image 20240612160603.png]]



