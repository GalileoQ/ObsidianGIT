La dirección MAC es un identificador único y permanente que se asigna a cada interfaz de red de un dispositivo. Es importante tener en cuenta que aunque la dirección MAC es única, se puede cambiar de forma temporal mediante software (como en este caso).

Para cambiar la MAC de un dispositivo Linux tenemos una herramienta que se llama macchanger, por tanto su primer comando será este para saber cual es la MAC de mi equipo:
![[Pasted image 20240612232005.png]]

Y ahora con este comando hacemos que nos cambie la MAC automáticamente, pero sólo cambiando la parte de la mitad a la derecha:
![[Pasted image 20240612232011.png]]

Si queremos cambiarla entera de manera random, utilizamos la opción -r:
![[Pasted image 20240612232015.png]]
Y ahora si queremos recuperar la MAC por defecto, la permanente, utilizamos la opción -p:
![[Pasted image 20240612232018.png]]

Por último, aquí tenemos el menú de ayuda para guiarnos por las opciones que trae esta herramienta:
![[Pasted image 20240612232023.png]]


