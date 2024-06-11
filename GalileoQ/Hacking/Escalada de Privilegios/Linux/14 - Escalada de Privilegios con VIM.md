Si ejecutamos el comando sudo -l podemos ver que el usuario www-data puede ejecutar cualquier comando que se encuentre dentro del directorio html o también creado con vim:
![[sdgdsfgsdfg.png]]
Ahora con vim podremos crear un código que nos mande una bash como root y lo podemos editar como sudo porque tenemos ese privilegio:
![[tgersghsdfghd.png]]
Pues ahora una vez abierto el fichero, en la parte de abajo de vim nosotros podemos establecer instrucciones, que por ejemplo para elevar privilegios escribiremos esto ya que somos usuario sudo:
![[dsgfhdfghdf.png]]
Y ahora a continuación si damos a enter se habrá guardado lo de antes; y si luego hacemos clic en escape y volvemos a hacer shift y dos puntos para escribir shell, ya nos lanzará una bash de root, por tanto damos a enter y lo tenemos:
![[Pasted image 20230401005330.png]]

-------------------------------------------------------------------

## OTRO EJEMPLO

Ejecutamos el comando sudo -l para ver que acciones podemos ejecutar con el usuario que somos; por ejemplo en este caso podemos ejecutar vim:
![[Pasted image 20230128140320.png]]
Ahora con vim podremos crear un código que nos mande una bash como root y lo podemos editar como sudo porque tenemos ese privilegio:
![[Pasted image 20230128140328.png]]
Pues ahora una vez abierto el fichero, en la parte de abajo de vim nosotros podemos establecer instrucciones, que por ejemplo para elevar privilegios escribiremos esto ya que somos usuario sudo:
![[Pasted image 20230128140338.png]]
Y ahora a continuación si damos a enter se habrá guardado lo de antes; y si luego hacemos clic en escape y volvemos a hacer shift y dos puntos para escribir shell, ya nos lanzará una bash de root, por tanto damos a enter y lo tenemos:
![[Pasted image 20230128140352.png]]
![[Pasted image 20230128140356.png]]



